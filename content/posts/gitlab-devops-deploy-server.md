---
title: "GitLab DevOps: Deploy to Server"
date: 2022-12-30T20:45:05.000Z
author: Ryan Robinson
description: "Deploying to servers as part of a GitLab DevOps workflow."
series: GitLab DevOps
tags:
  - Git, GitLab, and GitHub
  - DevOps
  - PHP
---

This continues with [the long-stalled GitLab DevOps series](/tags/gitlab-devops/). An essential piece of any CI/CD system is deploying to another server. You do not want to have to sign in to each server separately and carry out 15 deployment steps to take backups, pull the updated code, import new configuration, update database schemas, clear caches, etc. You want to hit a button and it rolls out everywhere. You also want to have the ability to easily roll back if you realize a mistake was made.

You will first need [a GitLab runner](/posts/2022/gitlab-devops-gitlab-runner/) on the server you want to deploy to.

As with the earlier post on [PHP linting within GitLab CI/CD](/posts/2022/gitlab-devops-php-lint/), in a real scenario this would be split into two projects. In my real usage, I have one project with the general CI/CD functionality, then the specific website project can extend from those functions to push out to the server. In this demo scenario, it is all in [one project of my GitHub](https://github.com/ryan-l-robinson/GitLab-CI-CD).

## Deploy.yml

The deploy file provides you a baseline template for deploying code changes, which is in the general CI/CD functionality project. Here's what that might look like:

```yml
## Deploy Jobs ##
variables:
  ENVIRONMENT_NAME: "dev"
  SERVER_URL: ""
  WEB_ROOT: "/opt/www/html"

### Generic deploy to specified server ###
.deploy_template:
  stage: deploy
  environment:
    name: $ENVIRONMENT_NAME
    url: $SERVER_URL
  script:
    - echo "Deploying to server at $SERVER_URL"
    - cd $WEB_ROOT
    - git fetch
    - git reset --hard
    - git stash
    - git pull
```

## Project .gitlab-ci.yml

The other key component is the project's file to extend this deploy job.

Start the project's .gitlab-ci.yml file with including the deploy file from the other project. In a scenario where that is a different project but accessible to the same GitLab user, it would look like this, assuming the project is called gitlab-ci:

```yml
# Includes general CI jobs
include:
  - project: "[group path to project]/gitlab-ci"
    ref: main
    file: deploy.yml
```

This will now give you the freedom to extend jobs found in the deploy.yml file of the main branch on the gitlab-ci project.

Add deploy as a stage for your project:

```yml
stages:
  - test
  - deploy
```

Finally, define the job(s) that use(s) the general deploy functionality, passing in the needed variables:

```yml
## Deploy Jobs ##

### Deploys to dev server only when changes are made to the dev branch ###
deploy_dev:
  extends:
    - .deploy_template
  variables:
    ENVIRONMENT_NAME: Development
    SERVER_URL: [dev url]
    WEB_ROOT: /opt/www/html
  tags:
    - [dev GitLab runner tag]
  only:
    refs:
      - dev

### Deploys to staging server only when changes are made to the main branch ###
deploy_staging:
  extends:
    - .deploy_template
  variables:
    ENVIRONMENT_NAME: Development
    SERVER_URL: [dev url]
    WEB_ROOT: /opt/www/html
  tags:
    - [staging GitLab runner tag]
  only:
    refs:
      - main

### Deploys to production only when main branch and manually triggered ###
deploy_prod1:
  extends:
    - .deploy_template
  variables:
    ENVIRONMENT_NAME: Production 1
    SERVER_URL: [production 1 URL]
    WEB_ROOT: /opt/www/html
  tags:
    - [prod 1 GitLab runner tag]
  only:
    refs:
      - main
  when: manual

deploy_prod2:
  extends:
    - .deploy_template
  variables:
    ENVIRONMENT_NAME: Production 2
    SERVER_URL: [production 2 URL]
    WEB_ROOT: /opt/www/html
  tags:
    - [prod 2 GitLab runner tag]
  only:
    refs:
      - main
  when: manual
```

You will need one of these types of jobs for each server to deploy to, with each's corresponding variables. This demo includes a dev server, a staging server, and two production servers.

## Project Access Token

For this to work smoothly, you can use a project access token. This is a unique token that can be set up on a project – essentially a service account – rather than the connection being tied to a particular user. This is helpful to avoid problem scenarios like a sudden change of staff when the deploy was set up using the former staff's account that no longer exists.

Create a project token by going to the project -&gt; Settings -&gt; Project Access Tokens. Name the token something descriptive for the server that will be using it, e.g. Dev Server. Specify the permissions for that token. Within this workflow I've been walking through here, that is only read access so it can pull within the deployment job. It does not need to be able to write back to the GitLab project, since code changes will always start at local and push through, never the other way around.

Now when you clone the project or add remote for the new server, add it using a variation on the https format rather than the SSH format: `https://oauth2:[token]@[GitLab project address]`

## Next: Drupal Configuration

This deploys code changes to the servers. You quite possibly need more actions to take place, like database updates, depending on what platform you're using. In my context of developing this for Drupal, there are several more steps needed. Those will be covered in the next post in this series.
