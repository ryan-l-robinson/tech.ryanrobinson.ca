---
title: "Drupal GitPod Container 2: .gitpod.yml"
date: 2022-01-28T13:17:00.000Z
author: Ryan Robinson
tags:
  - Drupal
  - Git, GitLab, and GitHub
  - DevOps
  - GitPod Drupal
  - Visual Studio Code
series: GitPod Drupal
---

This post picks up in a [mini-series](/tags/gitpod-drupal/) describing how I created a generic Drupal-friendly container working with [GitPod](https://gitpod.io). The [first covered the GitPod.Dockerfile](/posts/2022/drupal-gitpod-container-1-dockerfile/) to build the core LAMP stack image.

[The entire project is available in my GitHub](https://github.com/ryan-l-robinson/Drupal-GitPod).

More of the remaining work is done by the .gitpod.yml file, including referencing the Dockerfile image described in the previous part, as the starting point.

```yml
image:
  file: .gitpod/GitPod.Dockerfile
```

## Tasks

It then runs a handful of tasks, split into two sections. The tasks under “init” will run only when the container is created for the first time. The tasks under “command” will run every time the container is started, even if you’re re-opening an existing container.

### Create the database

Creating the database is straightforward and familiar enough to any who have used MySQL from a command line before:

```bash
sudo mysql -e "CREATE DATABASE IF NOT EXISTS drupal"
```

### Install codebase

The contributed codebase can be built using composer install:

```bash
composer install
```

I’m also going to copy the settings file into place:

```bash
sudo cp .gitpod/drupal.settings.php web/sites/default/settings.php
```

That settings file includes several alterations including where to find the configuration and content sync directories, how to connect to the database, and which URL is allowed to login from. Most of it will be familiar to those who have used Drupal settings files before, but I will cover anything unique in my next post for this series.

### Drupal Settings and Content

This is the hardest part of working with Drupal websites. All of the configuration and content is held in the database, not the codebase. Drupal (since 8) offers a configuration sync tool out of the box, and a content sync tool as a module. Broadly speaking, the configuration tool is usually pretty reliable, while the content tool has had some occasional glitches in my time using it.

I install the site using [the drush site-install tool](https://www.drush.org/latest/commands/site_install/):

```bash
vendor/drush/drush/drush -y site-install minimal --existing-config
```

The key there is the –existing-config tag. This starts the installation using the configuration details you already have in the configuration sync folder. The main thing this accomplishes is keeping the UUID of the site in sync. Config sync and content sync will not work if the UUIDs don’t match.

Now the content sync will also work:

```bash
vendor/drush/drush/drush -y content-sync:import --entity-types=block_content,file,node
```

The reason I added the –entity-types instead of simply importing all entity types was because of some experiences with [menu\_entity\_index](https://www.drupal.org/project/menu_entity_index). I won’t get into those details here, but that created another entity type for menu links, which tended to not synchronize very well mainly because the content sync does not sync the node ID of each content. That meant that a page might have a different ID on one installation from the content sync than on another. Menu items were then pointing to the node ID, which is now different content, and it created some nasty URL aliases.

Similarly related to the node ID sync is the homepage. The configuration is saved with specifying which piece of content is the homepage using the link to the node ID. But that might no longer be the content you want on the homepage. To solve this, I added a query to find my real homepage and set it again each time:

```bash
home_id=$(vendor/drush/drush/drush sql-query 'SELECT nid FROM node_field_data where type="home" and status="1" and title="Home" limit 1;')
if [[ ! -z ${home_id} ]]
then
    vendor/drush/drush/drush cset -y system.site page.front /node/$home_id
fi
```

The key for that structure to be maintained is that the homepage is of the content type “Home” and with the title of “Home.” If that isn’t met, this workaround will fail.

### Admin Password

By default, the site install at this point created a default super-admin user with a random password. For the purpose of this demo, I wanted to control what that admin password was so I could put it in the GitHub README and keep it saved in my [password manager](https://ryanrobinson.technology/all/tools/security-essentials-password-manager/). So, I used drush to always reset the admin password back to what I wanted it to be:

```bash
vendor/drush/drush/drush user:password admin "ZNB*ufm1tyz4rwc@yzk"
```

### Cleanup

There are two more small acts of clean up that I want, to minimize warning errors that would otherwise appear as soon as I sign in. One is to rebuild the content permissions and the other is to run cron.

```bash
vendor/drush/drush/drush php-eval 'node_access_rebuild();'
vendor/drush/drush/drush cron
```

### Drush Alias

I want to be able to use the command `drush` instead of always having to reference the full path to drush, as I did above. So I add an alias to my .bashrc. You can also do this by adding the location of drush to the PATH. Arguably that is more correct but this was simpler in a GitPod scenario where containers are constantly being built and destroyed and so don’t always have to be precisely the long-term ideal.

```bash
echo 'alias drush="/workspace/drupal-gitpod/vendor/drush/drush/drush"' >> ~/.bashrc
```

### Command

The command tasks accomplish two simple things: start Apache, and source the bashrc to make sure it notices the drush alias.

```bash
source ~/.bashrc
apachectl start
```

## Ports

The GitPod.yml file optionally defines ports and what to do when they open. You don’t need to have this section. The ports will open either way. But it can be nice to specify in advance what you want to do when each port opens. In this case, I’m ignoring everything except for 8001, which I will open in the browser, since that is the port for the Apache access. If you don’t specify, it will give you a popup with each port opened asking what you want to do, so this saves some time.

```gitpod
ports:
  - port: 8001
    onOpen: open-browser
  - port: 8828
    onOpen: ignore
  - port: 8829
    onOpen: ignore
  - port: 3306
    onOpen: ignore
```

## GitHub

This section starts with a somewhat misleading key “github.” These functions are not exclusive to GitHub – they can be used with any of the supported version control platforms with GitLab and Bitbucket as well. The main valuable thing here to me is defining prebuilds, which enables GitPod to start building the new image as soon as code changes are submitted to the repository. That saves time when you’re ready to start working and it’s already good to go, instead of waiting a few minutes.

```gitpod
github:
  prebuilds:
    master: true
    branches: true
    pullRequests: true
    pullRequestsFromForks: true
    addCheck: true
    addComment: false
    addBadge: true
    addLabel: true
```

## Extensions

The final section of this file is to install [VS Code](/tags/visual-studio-code/) extensions. There is one big qualifier here: it can only install extensions that are available in [the Open VSX registry](https://open-vsx.org/). This may not be everything available for VS Code, with the most notable exclusion for me being [GitHub Copilot](/websites/github-copilot/).

Here’s my Drupal 9 friendly set of extensions:

```gitpod
vscode:
  extensions:
    - eamodio.gitlens
    - skippednote.VS-code-drupal
    - cweijan.vscode-mysql-client2
    - esbenp.prettier-vscode
    - gruntfuggly.todo-tree
    - mblode.twig-language
    - vscode-icons-team.vscode-icons
```

## Almost There…

In the last post of this series, I’ll look at a few other configuration changes I needed to make for this to work.