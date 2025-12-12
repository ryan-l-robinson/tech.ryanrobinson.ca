---
title: "Drupal Docker: devcontainer"
date: 2022-03-18T12:38:00.000Z
author: Ryan Robinson
description: "devcontainer configuration for a Drupal local environment."
series: Drupal Docker
tags:
  - Drupal
  - PHP
---

I have previously shared [setting up local development environments using vagrant](/posts/2021/vagrant-oracle-linux-vm/) and [GitPod](/tags/gitpod-drupal/) in Drupal friendly ways. This post will start a new mini-series on how I built a Docker Desktop setup for a Drupal-friendly environment that is (mostly) based on Oracle Linux 8.

Code for this is found in [my GitHub](https://github.com/ryan-l-robinson/Drupal-Devcontainer).

## Overview

There will need to be several files to make this work:

- devcontainer.json for how to integrate with [VS Code](/tags/visual-studio-code/)
- Docker-compose.yml for how to tie together all the different containers
- web.Dockerfile for the primary Apache container (Oracle Linux based)
- php.Dockerfile for the PHP-FPM container (Oracle Linux based)
- db.Dockerfile for the MariaDB database container (a simpler image from an official MariaDB image)

## Devcontainer.json

I’ll start at the first file which gets called when you open VS Code: the .devcontainer/devcontainer.json.

The most important pieces are near the top. This includes:

Where to find the Docker composer file:

```json
  "dockerComposeFile": "../docker-compose.yml",
```

What user to connect as:

```json
  "remoteUser": "drupal",
```

What service to connect to and what other services to start up in the background:

```json
  "service": "web",
  "runServices": ["web", "php", "db"],
```

In my case, my primary is called web for the main Apache container, with two other services for PHP and DB (MariaDB).

What location on the container should be your workspace after connecting:

```json
  "workspaceFolder": "/var/www/html",
```

You can also define what extensions and settings should be installed in this container. Unlike [the GitPod equivalent](/websites/drupal/drupal-gitpod-container-2-gitpod-yml/), this can be any VS Code extension, not only the Open VSX ones. I’ve written [some of my favourite extensions in the past](/websites/favourite-visual-studio-code-extensions/), but this is a good sample of some I use specifically with Drupal:

```json
    "extensions": [
      "gruntfuggly.todo-tree",
      "eamodio.gitlens",
      "gitkraken.gitkraken-authentication",
      "cweijan.vscode-mysql-client2",
      "esbenp.prettier-vscode",
      "whatwedo.twig",
      "marcostazi.vs-code-drupal",
      "github.copilot",
      "vscode-icons-team.vscode-icons",
      "xdebug.php-debug",
    ],
```

### Scripts

You can trigger other scripts to run at various points in the process.

The primary one I made use of is postCreateCommand. This will run only once when the container is first built. This one is much more complicated building the entire site. I’ll have another post dedicated to that script.

```json
    "postCreateCommand": "/bin/bash -c \"/postCreateCommand.sh\"",
```

I also have a much simpler postAttachCommand. This only sources the ~/.bashrc to ensure the user knows where to find commands like drush and composer, which were set up by the postCreateCommand script.

```json
    "postAttachCommand": "/bin/bash -c \"source ~/.bashrc\"",
```
