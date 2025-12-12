---
title: "Drupal Dev Environment Updated: The DevContainer"
date: 2024-04-08T15:46:00.000Z
author: Ryan Robinson
description: "An updated version of the devcontainer for a Drupal dev environment."
series: Drupal Docker
tags:
    - Drupal
    - DevOps
---

This continues from [a previous post](/posts/2024/dev-env-updated-dockerfiles/) about a new Drupal dev environment, a significant upgrade from some previous posts that you can find on [the Drupal Docker tag](/tags/drupal-docker/). That covered the Dockerfiles, while this will cover changes to the Devcontainer.json and the postCreateCommand script. [The whole project's code can be found on my GitHub](https://github.com/ryan-l-robinson/Drupal-Devcontainer).

## Devcontainer

What changed in devcontainer from the previous setup? A few things:

- I reviewed several of the extensions and made some changes. The most significant are adding Playwright testing (maybe I'll be able to write more about that later), the PHP tools extension, and the Drupal extension. Between them I now have much stronger error detection, deprecation warnings, testing, and formatting to meet the Drupal standards. Some more notes about useful extensions are in [my recent post about what extensions I use](/websites/vs-code/extensions-2024/).
- The postAttachCommand has to set the permissions again with each new time attaching to it, so the files that are set up as volumes don't revert to root.

## postCreateCommand

This is mostly the same, at least for now: it installs all composer packages and sets up the database on the first time creating these images. A few things were able to be removed to simplify this script, functions like creating the database that are handled more easily in other ways. 

What it still does:

- Installs composer packages.
- Puts the Drupal settings and local services files in place. This comes after the composer install so that the basic Drupal file structure is there already.
- Import the site's configuration using drush.
- Set the admin password.
- Rebuild the node access cache. There aren't any nodes yet, but there is a warning on the site status for a bit if this function isn't included, so it's one extra command to save some confusion.
- Sets the environment indicator theme.
- Clears the site's caches to reflect all changes.

## Next: GitLab/GitHub CI/CD Updates

I'm going to continue with this by now improving it a step farther: instead of building the images each time on local, have it build in GitHub or GitLab programmatically, and then saving it alongside the repository in those platforms' respective container registries. Then when it is time to start a new local development environment, it only needs to pull the latest image, not build it again. That saves time, especially if you have to switch between branches and especially if you have multiple team members who would otherwise all be spending time building it individually. This will include moving most of the postCreateCommand functionality into the GitLab CI / GitHub Actions, for both the web and database containers.

I already have this working in my real context using GitLab. I have less experience with GitHub Actions but am working on doing the same idea there for a more shareable version.
