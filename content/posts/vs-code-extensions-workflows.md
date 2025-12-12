---
title: "VS Code Extensions: My Workflows"
date: 2024-02-21T14:31:00-04:00
author: Ryan Robinson
description: "An overview of which VS Code extensions I use for different workflows."
tags:
  - Visual Studio Code
  - Drupal
---

When it comes to my VS Code extensions, I have mine set up a few different ways depending on what kind of project I'm working on:

## Default

These are the default plugins I have locally installed, for every profile and every project container.

- [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
- [Playwright Tests for VS code](https://marketplace.visualstudio.com/items?itemName=ms-playwright.playwright)
- [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- [Todo Tree](https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.todo-tree)
- [tl;dr pages](https://marketplace.visualstudio.com/items?itemName=bmuskalla.vscode-tldr)
- [vscode-icons](https://marketplace.visualstudio.com/items?itemName=vscode-icons-team.vscode-icons)

## Static Sites

These are additional plugins which come in handy with building static sites like this one and ryanrobinson.ca:

- [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)
- [Markdown lint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint)
- [Front Matter CMS](https://marketplace.visualstudio.com/items?itemName=eliostruyf.vscode-front-matter)

## Containers/Remote (WordPress/Drupal)

This profile includes the basic requirements to connect to containers and/or remote:

- [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
- [Remote SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)

Each container, using the devcontainer.json functionality, can also define new extensions for that container. So here's an example of doing that for a Drupal site:

- [MySQL](https://marketplace.visualstudio.com/items?itemName=cweijan.vscode-mysql-client2)
- [Drupal](https://marketplace.visualstudio.com/items?itemName=Stanislav.vscode-drupal)
- [Twig](https://marketplace.visualstudio.com/items?itemName=whatwedo.twig)
- [PHPTools](https://marketplace.visualstudio.com/items?itemName=DEVSENSE.phptools-vscode)
