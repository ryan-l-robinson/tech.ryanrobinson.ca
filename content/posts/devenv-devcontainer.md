---
title: "VS Code DevContainer"
date: 2025-12-26T15:46:00.000Z
author: Ryan Robinson
description: "The devcontainer file specifies how VS Code will run this environment, including extensions, settings, and scripts."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
  - Linux
draft: true
---

This post is one of several in a series detailing my development environment and CI/CD deployment pipeline. [The code for the developer environment can be seen on my GitLab](https://gitlab.com/ryan-l-robinson/drupal-dev-environment). Other posts in the series can be seen by checking the Drupal Docker series links in the sidebar. I provided [an overview in the first post of the series](/2025/drupal-docker-deploys-overview/).

We now have a robust Dockerfile for the main web image and two layers of docker-compose files. Now we can tell VS Code how to use those files to build the environment we need. To do this, create a file named devcontainer.json inside a .devcontainer folder in the project.

## The Essentials

The most essential part is at the beginning, where it tells VS Code what services it needs to run, based on what docker-compose files, and running as what user.

```json
  "name": "drupal.devenv",
  "dockerComposeFile": ["../docker-compose.yml", "docker-compose.yml"],
  "remoteUser": "root",
  "service": "web",
  "runServices": ["web", "db"],
  "workspaceFolder": "/opt/drupal",
```

In this case, I'm naming it drupal.devenv, opening the two docker-compose files together, logging in with the root user on the web service at /opt/drupal, but also running the database service.

## Extensions

These extensions help lock in a variety of extensions that make sense for this project. These could instead be done by each user on their machine, but then it doesn't share the settings with others just starting up using the project; they would have to figure out all the settings themselves. They also could be put into .vscode/extensions.json and committed to the project that way; that works but the extensions are only recommended instead of automatically installed and will also apply when opening the repository locally even when some of them only make sense when within a functioning container environment. This way shares with all users but only within the built environment.

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

This has a similar dynamic as extensions, where they could be done other ways, but for ease of being shared and setup for all users, it makes the most sense here.

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

Finally, the devcontainer can specify scripts at various stages of the process. There is a separate file to run on first creation of a new developer environment. When attaching to the environment each time, reset the permissions, as they sometimes get lost as a volume. The last line simply tells it that when the VS Code environment closes, it should also stop the services spun up with the docker-compose files.

```json
  "postCreateCommand": "/bin/bash -c /opt/drupal/scripts/postCreateCommand.sh",
  "postAttachCommand": "/bin/bash -c \"chown -R www-data:www-data /opt/drupal\"",
  "shutdownAction": "stopCompose"
```
