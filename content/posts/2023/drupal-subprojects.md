---
title: "Composer: Options for a Submodule"
date: 2023-07-09T14:12:00-05:00
author: Ryan Robinson
category: Websites
tags:
  - Drupal
  - Git, GitLab, and GitHub
---

Suppose you want to tie in another private git package, such as something that is not listed on the Drupal directory - as part of your standard build process. Here are two approaches to do that:

## Composer

Drupal packages are managed with a composer file. Composer files can pull from many sources. Most commonly in the case of a Drupal project, that means pulling in modules and themes from the Drupal directory. But it also includes the ability to pull in projects as zip files or a git project hosted elsewhere, even a private one.

Here's how:

Create the module that you want to pull in and give it a composer.json file. Here's an example of that file:

```json
{
  "name": "ryan-robinson/demo-module",
  "type": "drupal-module",
  "description": "Demo module",
  "keywords": [
    "Drupal",
    "module"
  ],
  "license": "GPL-2.0+",
  "require": {
    "drupal/core": "^9.0 || ^10"
  },
  "config": {
    "platform": {
      "php": "8.1"
    }
  }
}
```

Add two things to the composer.json of your Drupal project:

One, you need to define the source for where to get the project:

```json
    "repositories": [
        {
            "type": "vcs",
            "url": "git@gitlab.com:ryan-robinson/demo-module.git",
            "branch": "main"
        },
    ]
```

Second, you need to add it to the requirements list to pull in:

```json
    "require": {
      "ryan-robinson/demo-module": "dev-main"
    }
```

If the repository is private, you'll also need an auth.json file with your personal access token. Here's an example that would work for a GitLab project:

```json
{
    "gitlab-token": {
        "gitlab.com": "token value"
    }
}
```

## Git Submodule

The other approach is to make it a git submodule.

The advantage of the submodule approach is if you want to be able to continue to contribute to that. You can work on the parent project and the submodule at the same time, committing them both separately. Of course that only matters I'd you have permission to do so.

The context I've found myself using this: when I'm developing a custom Drupal module, I need a Docker devcontainer environment to work within for testing. That's one git repository. I also need a repository for the module itself, so that it will be ready to share with other Drupal projects (often using the composer technique above). This allows me to load the devcontainer, work on the module, and commit back to the module.

This can be done by running the `git submodule add` command, e.g.:

```bash
git submodule add [URL to module]
```

This generates a .gitmodules file at the project root that looks something like this:

```
[submodule "web/modules/custom/demo-module"]
	path = web/modules/custom/demo-module
	url = [URL to module]
```
