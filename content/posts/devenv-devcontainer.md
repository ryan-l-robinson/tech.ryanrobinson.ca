---
title: "VS Code DevContainer"
date: 2025-12-23T14:16:00.000Z
author: Ryan Robinson
description: "The devcontainer file specifies how VS Code will run this environment, including extensions, settings, and scripts."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
  - Linux
  - Visual Studio Code
---

This post is one of several in a series detailing my development environment and CI/CD deployment pipeline. [The code for the developer environment can be seen on my GitLab](https://gitlab.com/ryan-l-robinson/drupal-dev-environment). Other posts in the series can be seen by checking the Drupal Docker series links in the sidebar. I provided [an overview in the first post of the series](/2025/drupal-docker-deploys-overview/).

In [the last post](/2025/devenv-docker-compose/), I walked through setting up the docker-compose files to define which services are needed, with some variables by environment and slit into two files so that some is only for local development. Now, we can move on to defining how VS Code knows to use those docker-compose files as well as some other features configuring the environment. This is done using a file named devcontainer.json inside a .devcontainer folder in the project.

## The Essentials

The most essential part is at the beginning, where it tells VS Code what services it needs to run, based on what docker-compose file(s), and running as what user.

```json
  "name": "drupal.devenv",
  "dockerComposeFile": ["../docker-compose.yml", "docker-compose.yml"],
  "remoteUser": "root",
  "service": "web",
  "runServices": ["web", "db"],
  "workspaceFolder": "/opt/drupal",
```

In this case, I'm naming it drupal.devenv, opening the two docker-compose files together (that's what makes it different than a deployment to other servers), logging in with the root user on the web service at /opt/drupal, but also running the database service.

Logging in with the root user is the most hassle here. Some things don't work easily as root. Other things don't work at all as anything other than root. I'd still like to change this in theory to logging in with www-data, the Apache user, and simplify some other things, but at least at the time it was determined to not be possible.

## Extensions

The extensions section help lock in a variety of VS Code extensions that make sense for this project.

These could instead be done by each user on their machine, but then it doesn't share the settings with others just starting up using the project; they would have to figure out all the settings themselves. The extensions also could be put into .vscode/extensions.json and committed to the project that way; that works but the extensions are only recommended instead of automatically installed and will also apply when opening the repository on the host machine even when some of them only make sense within a functioning container environment. This way shares with all users but only within the built environment, which is what we want for this kind of project.

```json
  "customizations": {
    "vscode": {
      "extensions": [
        "gitlab.gitlab-workflow",
        "gruntfuggly.todo-tree",
        "eamodio.gitlens",
        "gitkraken.gitkraken-authentication",
        "cweijan.vscode-mysql-client2",
        "esbenp.prettier-vscode",
        "Stanislav.vscode-drupal",
        "SanderRonde.phpstan-vscode",
        "whatwedo.twig",
        "vscode-icons-team.vscode-icons",
        "DEVSENSE.phptools-vscode",
        "bmuskalla.vscode-tldr",
        "ms-playwright.playwright",
        "saoudrizwan.claude-dev",
        "recca0120.vscode-phpunit"
      ]
    }
  }
```

## Settings

This has a similar dynamic as extensions, where they could be added by each user separately or could be added in the project, but for ease of being shared and setup for all users, it makes the most sense here as part of the devcontainer configuration.

Some of the settings a change here, which range from personal preference to allowing a useful function:

- Tells it where to find php to run.
- Disables the inlay hints of parameters, which I found distracting.
- Enforced Drupal standard formatting using an extension.
- Encourages Drupal best practice style with PHP CodeSniffer.
- Defined that phpunit tests run in the Tests Explorer need to use a different wrapper script. That is used to force tests to run as www-data instead of as root, one of the problems I mentioned above from needing to run as root otherwise.
- Sets bash as the default terminal.
- Sets a colour palette.

```json
    "vscode": {
      "settings": {
        "php": {
          "executablePath": "/usr/local/bin/php",
          "suggest.basic": true,
          "inlayHints.parameters.enabled": false
        },
        "[php]": {
          "editor.defaultFormatter": "stanislav.vscode-drupal"
        },
        "phpcs": {
          "standard": "Drupal,DrupalPractice",
          "executablePath": "/opt/drupal/vendor/bin/phpcs"
        },
        "phpunit.php": "/opt/drupal/scripts/phpunit-wrapper.sh",
        "terminal": {
          "external.linuxExec": "/usr/bin/bash",
          "integrated.defaultProfile.linux": "bash"
        },
        "workbench": {
          "editor.highlightModifiedTabs": true,
          "colorCustomizations": {
            "activityBar.activeBackground": "#000000",
            "activityBar.activeBorder": "#fabd0f",
            "activityBar.background": "#000000",
            "activityBar.foreground": "#ffffff",
            "activityBar.inactiveForeground": "#ffffff",
            "activityBarBadge.background": "#fabd0f",
            "activityBarBadge.foreground": "#002452",
            "debugIcon.breakpointForeground": "#b90e31",
            "editorGroup.border": "#fabd0f",
            "panel.border": "#fabd0f",
            "sideBar.border": "#fabd0f",
            "titleBar.activeBackground": "#000000",
            "titleBar.activeForeground": "#ffffff",
            "titleBar.inactiveBackground": "#000000",
            "titleBar.inactiveForeground": "#ffffff",
            "sash.hoverBorder": "#b90e31",
            "statusBar.background": "#000000",
            "statusBar.foreground": "#ffffff",
            "statusBar.debuggingBackground": "#002452",
            "statusBar.debuggingForeground": "#ffffff",
            "statusBarItem.hoverBackground": "#b90e31",
            "statusBarItem.remoteBackground": "#002452",
            "statusBarItem.remoteForeground": "#ffffff"
          }
        }
      }
    }
```

I am routinely tinkering with these settings, so what I wrote here may not be the same forever as what's in the GitLab project.

## Scripts

Finally, the devcontainer can specify scripts to run at various stages of the process.

There is a separate file to run on first creation of a new developer environment, which I will get to as the next post in this series.

When attaching to the environment each time, I have it reset the permissions as they sometimes get lost as a volume.

The last line simply tells it that when the VS Code environment closes, it should also stop the services spun up with the docker-compose files.

```json
  "postCreateCommand": "/bin/bash -c /opt/drupal/scripts/postCreateCommand.sh",
  "postAttachCommand": "/bin/bash -c \"chown -R www-data:www-data /opt/drupal\"",
  "shutdownAction": "stopCompose"
```
