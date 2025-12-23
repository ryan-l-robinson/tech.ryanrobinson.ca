---
title: Pruning Old Docker Objects in CI/CD
date: 2025-12-23T14:30:16.700Z
author: Ryan Robinson
description: "A simple job to prune old Docker objects on a runner."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
---

This post is one of several in a series detailing my development environment and CI/CD deployment pipeline. [The code for the developer environment can be seen on my GitLab](https://gitlab.com/ryan-l-robinson/drupal-dev-environment). Other posts in the series can be seen by checking the Drupal Docker series links in the sidebar. I provided [an overview in the first post of the series](/2025/drupal-docker-deploys-overview/).

In [a previous post](/2025/devenv-build-image-ci/), I detailed using a CI/CD runner to build the image and store it in the container registry. If you're using GitLab.com, that's probably fine as is. If you have your own runner on a different server that could run out of storage, though, you might want to add some cleanup tasks to keep from filling it up.

As with most other reusable GitLab CI/CD jobs, I have this structured as a general purpose job in [my GitLab CI project](https://gitlab.com/ryan-l-robinson/drupal-dev-environment), and other projects able to implement it.

## The Extendable Job

This is the general purpose job:

```yml
## This is used to prune dangling Docker pieces older than UNTIL_HOURS hours, to cut down on wasted space. ##
.docker_prune:
  stage: deploy
  variables:
    UNTIL_HOURS: 8760 #roughly a year
  retry:
    max: 2
    when:
      - script_failure
  script:
    - docker system prune -af --filter "until=${UNTIL_HOURS}h"
```

It is a straightforward command, pruning all types of objects older than a certain number of hours. It will retry a couple of times if it fails the first time.

## Per Project Implementation

Then your project's .gitlab-ci.yml only needs something simple like this:

```yml
# Includes general CI jobs #
include:
  - project: "ryan-l-robinson/gitlab-ci"
    ref: main
    file: deploy.yml

build_prune:
  extends: .docker_prune
  stage: build
  variables:
    UNTIL_HOURS: 168
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
      when: never
    - if: "$CI_COMMIT_BRANCH =~ /^202/ || $CI_COMMIT_BRANCH =~ /^(dev|staging|main)$/"
    - changes:
        - .devcontainer/*
        - .devcontainer/**/*
        - .gitlab-ci.yml
        - composer.lock
        - Dockerfile.drupal
        - scripts/*
        - web/modules/custom/**/*
        - web/themes/custom/**/*
  tags:
    - build-docker
```

The tag is important and must be the same as the one that you're using to build the images. Otherwise, you're pruning the wrong server, which might not even have Docker on it.

The rules for when it runs should probably be the same as the rules set up for the build job, since you want to prune enough to be sure you've got room to do the build. It could also be a bit more expansive, running more often, because it is such a quick job it won't slow things down much, but it also isn't really necessary.
