---
title: Pruning Old Docker Objects in CI/CD
date: 2025-12-28T22:54:16.700Z
author: Ryan Robinson
description: "A simple job to prune old Docker objects on a runner."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
draft: true
---

You may find as you are doing deployments of Docker images that you start to use up a lot of space on servers for all those old images and containers. To help with this, I developed a simple job that would prune old images, running before any new deployment.

This is the core of the job, defined in [a central project that all projects can reference](https://gitlab.com/ryan-l-robinson/gitlab-ci/-/blob/main/deploy.yml):

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
        - apache/**/*
        - composer.lock
        - Dockerfile.drupal
        - php/*
        - phpunit.xml.tmpl
        - scripts/*
        - settings.php.tmpl
        - web/modules/custom/**/*
        - web/themes/custom/**/*
  tags:
    - build-docker
```

In this example, the prune happens for any commits to a release branch - those that start with 202 because the release branches are named as YYYY-MM - as well as dev, staging, and main. It also runs for changes to certain files, the ones that are most likely to require a newly built image even if it is an issue branch rather than those other branches that always get a new build. The tag must be the one for the runner on the server that you want to clean up.
