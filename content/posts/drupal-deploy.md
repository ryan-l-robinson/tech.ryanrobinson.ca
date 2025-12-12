---
title: "GitLab DevOps: Drupal Deployment"
date: 2023-01-02T15:47:05.000Z
author: Ryan Robinson
description: "How to deploy a Drupal site to servers using GitLab CI/CD."
series: GitLab DevOps
tags:
  - Drupal
  - Git, GitLab, and GitHub
  - PHP
---

This continues where [the previous post](/posts/2023/drupal-deploy/) in the GitLab DevOps series left off. We can now deploy code changes to the new server, and that's great for generic deployments. Drupal adds a few extra components when it comes to configuration sync and the database.

## Dev vs Production

The biggest challenge with this topic is navigating that there should be some things different from dev and staging servers compared to production. Some of these are cases where the module or core configuration is on each server, but with different options (e.g. [environment indicator](https://drupal.org/project/environment_indicator)) and others are a case of modules that are only needed on dev and staging and therefore shouldn't be on production at all (e.g. [maillog](https://www.drupal.org/project/maillog) and [stage_file_proxy](https://www.drupal.org/project/stage_file_proxy)).

Composer offers a good way to handle the code package building distinction. You can require a package, or require-dev a package. Then you can `composer install` or `composer install --no-dev` depending on the server. That part is straightforward.

The hard part is the distinctions in configuration. Much of this can be handled using the [config_ignore](https://www.drupal.org/project/config_ignore) module, which tells the configuration imports and exports to not include that configuration file, which means it doesn't get synced across the servers. You have to set those ignored configurations up separately on every server, which means if there's a change, you need to remember to do it on all environments where it would be relevant. But then there's other configurations like modules that shouldn't be active at all on production but is on dev and staging. You can't simply ignore syncing the installed modules configuration, or else you'd have a mess every time you added or removed a module. Sometimes those modules create new configuration schemas, or require you to change another configuration schema to take advantage of it.

Maillog is the best example of this. Maillog is a module that should install on dev/staging but not production, as it is a debugging tool for logging outgoing mail rather than sending it. It can be separated in composer as a dev module so that the code doesn't install on production. But then it also changes the modules configuration when it is installed, as well as the site's mail settings configuration. Ideally these servers are still managed through the configuration, with differences in dev/staging compared to production. That rules out using config_ignore.

The other imperfect option is changing those configurations as part of the CI/CD processes. It also has a couple of problems:

1. There might be a few seconds where the configuration is wrong, e.g. if you import the synced configuration but then force it back to the desired value with a follow-up command. Often this doesn't matter, like a module being installed and then being promptly uninstalled again, but in other cases like sending email, it might be that the site tries to send an email in those few seconds, fails, and nobody notices that somebody should have gotten an email but didn't. So you'd have to be very careful about this technique in production.
2. It relies on keeping the CI/CD up to date. If something changes with the site configuration, you better remember to account for how that configuration will be the same or different on all servers and if the CI/CD needs any changes to prepare for it. This makes it more prone to something breaking when you forget or can't adequately test production behaviour until it's too late, already on production.

Note: I did try the [config_split](https://www.drupal.org/project/config_split) module, which sounds like it should help but did not work reliably. There is an update that came out since my last times, so maybe it will be worth trying it again.

For the simplicity of the rest of this blog post, I've decided not to include my specific examples here. Hopefully my thoughts on how I've negotiated the options for these kinds of scenarios is more valuable than any specific conclusions that we have in place right now.

## The Jobs

These jobs are added to the general function deploy.yml, which in my real-world scenario is in a different project but could be in the same project.

Here's what the extendable jobs look like:

```yml
### Generic deploy to specified server ###
.deploy_template:
  stage: deploy
  environment:
    name: $ENVIRONMENT_NAME
    url: $SERVER_URL
  before_script:
    - echo "Deploying to server at $SERVER_URL"
    - cd $WEB_ROOT
    - git fetch
    - git reset --hard
    - git stash
    - git pull

### Drupal install jobs ###
.drupal_composer:
  stage: deploy
  environment:
    name: $ENVIRONMENT_NAME
    url: $SERVER_URL
  script:
    - cd $WEB_ROOT
    - vendor/drush/drush/drush state:set system.maintenance_mode 1 --input-format=integer
    - composer install

.drupal_composer_prod:
  stage: deploy
  environment:
    name: $ENVIRONMENT_NAME
    url: $SERVER_URL
  script:
    - cd $WEB_ROOT
    - vendor/drush/drush/drush state:set system.maintenance_mode 1 --input-format=integer
    - composer install --no-dev

.drupal_config:
  stage: deploy
  environment:
    name: $ENVIRONMENT_NAME
    url: $SERVER_URL
  after_script:
    - cd $WEB_ROOT
    - vendor/drush/drush/drush state:set system.maintenance_mode 1 --input-format=integer
    - vendor/drush/drush/drush cr
    - vendor/drush/drush/drush config-import -y
    - vendor/drush/drush/drush cr
    - vendor/drush/drush/drush updb -y
    - vendor/drush/drush/drush state:set system.maintenance_mode 0 --input-format=integer
    - vendor/drush/drush/drush cr

.drupal_cache:
  stage: deploy
  environment:
    name: $ENVIRONMENT_NAME
    url: $SERVER_URL
  tags:
    - prod2
  after_script:
    - cd $WEB_ROOT
    - vendor/drush/drush/drush cr
```

The first job is the one detailed in [the previous post](/posts/2023/drupal-deploy/), except that it has now been moved from `script` to `before-script` to work better with the other pieces. 

The next component is composer. Drupal is built using composer packages. Any action that updates the composer.lock file - adding a new module, deleting a module, updating packages - will require this step. `composer install` ensures that it installs the exact same versions of the exact same packages as on your other servers. It might be tempting to use `composer update` instead to get the latest versions, but then you might end up with trying to install something that you haven't tested elsewhere yet. There are two variants of this job, one for dev servers that installs dev packages and one for production that does not.

Secondly, the configuration and databases need to be updated. `drush cr` rebuilds your caches. There's one to start this section because it is often necessary for scenarios like a new module. The Drupal cache needs to know when there's a new module with the code in place, before you get to the `config-import` that will try to install that module. Otherwise you'll get an error. With that settled, you can import all the configuration changes, then clear the caches again to ensure the site is now reflecting those changes. Finally, update the site's database using `drush updb`. This is sometimes needed with new modules or updated modules that need to change the database schema. If you don't do this, you can end up with errors about missing columns in tables.

The final job only clears the caches, without any of the other changes. The reason for these separations will become more clear in the site's jobs that extend from these.

## Site Jobs

These are the jobs to execute each job on the relevant server, found in the .gitlab-ci.yml file for the project.

```yml
## Deploy Jobs ##

### Deploys to dev server only when changes are made to the dev branch ###
deploy_dev:
  extends: 
    - .deploy_template
    - .drupal_composer
    - .drupal_config
  variables:
    ENVIRONMENT_NAME: Development
    SERVER_URL: dev.demo.com
    WEB_ROOT: /opt/www/html
  tags:
    - dev
  only:
    refs:
      - dev

### Deploys to staging server only when changes are made to the main branch ###
deploy_staging:
  extends:
    - .deploy_template
    - .drupal_composer
    - .drupal_config
  variables:
    ENVIRONMENT_NAME: Staging
    SERVER_URL: staging.demo.com
    WEB_ROOT: /opt/www/html
  tags:
    - staging
  only:
    refs:
      - main

### Deploys code to production only when main branch and manually triggered ###
deploy_prod_1:
  extends: 
    - .deploy_template
    - .drupal_composer
  variables:
    ENVIRONMENT_NAME: Prod 1
    SERVER_URL: prod1.demo.com
    WEB_ROOT: /opt/www/html
  tags:
    - prod1
  only:
    refs:
      - main  
  when: manual

deploy_prod_2:
  extends: 
    - .deploy_template
    - .drupal_composer
  variables:
    ENVIRONMENT_NAME: Prod 2
    SERVER_URL: prod2.demo.com
    WEB_ROOT: /opt/www/html
  tags:
    - prod2
  only:
    refs:
      - main  
  when: manual

#### Deploys config updates to production. These need to wait for both servers to have the composer updated code. ####
drupal_install_prod1:
  extends: .drupal_config
  variables:
    ENVIRONMENT_NAME: Production 1
    SERVER_URL: prod1.demo.com
    WEB_ROOT: /opt/www/html
  tags:
    - prod1
  only:
    refs:
      - main
  needs: [deploy_prod1, deploy_prod2]

drupal_install_prod2:
  extends: .drupal_cache
  variables:
    ENVIRONMENT_NAME: Production 2
    SERVER_URL: prod2.demo.com
    WEB_ROOT: /opt/www/html
  tags:
    - prod2
  only:
    refs:
      - main
  needs: [deploy_prod1, deploy_prod2, drupal_install_prod1]
```

Deploying to dev server can be done in a single job. The before_script deploys the code changes, the script installs from composer, and the after_script imports the configuration. It runs on any change to the dev branch. The idea is that we do work on separate issue branches, then when we need to test on dev, we merge it into the dev branch and it will deploy there.

Deploying to staging server is the same, but with different variables for the server details and activating on merges to main instead of to dev. Merges to main should only happen when its a release candidate ready for final testing before deployment.

Deploying to production adds a couple more layers to consider. There are two servers load balanced, with one database on a different dedicated server. Composer needs to install the updates on both servers. But the configuration only needs to be imported on one, since it's the same database - it wouldn't break anything to do the config on both, but would be an unnecessary extra few minutes of the site being in maintenance mode to run the same job all over again. The second server does still need caches cleared, though, or it might take some time for the changes to be reflected there, which could mean errors in the meantime.

The other factor to consider is that the configuration import should not run until the code update has been deployed on both servers. If the order of operations was to install code updates on prod 1, then run config import, it would be trying to make updates on the shared database when only one of the servers has the code ready for the updates. That's why production can't collapse to one job per environment like dev and staging do.

The resulting order of operation that you need is:

1. Deploy code changes, with composer install, to production 1.
2. Deploy code changes, with composer install, to production 2.
3. Import configuration changes to production 1.
4. Clear the caches on production 2.