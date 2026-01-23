---
title: Building the Dockerfile in CI/CD
date: 2025-12-23T12:46:16.700Z
author: Ryan Robinson
description: "How to build a project's Docker image in a GitLab CI/CD job and store it in the GitLab container registry."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
  - Linux
---

This post is one of several in a series detailing my development environment and CI/CD deployment pipeline. [The code for the developer environment can be seen on my GitLab](https://gitlab.com/ryan-l-robinson/drupal-dev-environment). Other posts in the series can be seen by checking the Drupal Docker series links in the sidebar. I provided [an overview in the first post of the series](/2025/drupal-docker-deploys-overview/).

We now have [a robust Dockerfile for the main web image](/2025/devenv-dockerfiles/) and [some scripts to run when it starts up](/2025/devenv-start-script/). There is one more essential step before we can start getting into details of the local development environment: we need to build that image we've defined and store it in the GitLab container registry where everyone with permission can access it.

To handle this, I have these jobs set up in two different places. One creates a general build job. Then it can be implemented per project. If you only have one project you need to build, you could collapse these into one job. If you have multiple projects needing building, though, it makes sense to split it up with a general template that all projects can use, rather than needing to repeat that in every project.

## The Build Template Job

The general build template job has a script which logs in to the GitLab container registry of the same project, builds the image including with any passed-through arguments, pushes it to the container registry, and then logs out again.

Also note some default variables such as those that include any git submodules. You may not need that in all projects, but I have needed it for several.

```yml
## General job for building from a Dockerfile. ##
.build_dockerfile:
  stage: build
  image: docker:dind
  services:
   - docker:dind
  variables:
    DOCKER_BUILDKIT: 1
    IMAGE_TAG: web
    # Defines image tag to go into the project's container registry, with a tag of the branch name. #
    FULL_CONTAINER_PATH: $CI_REGISTRY_IMAGE/$IMAGE_TAG:$CI_COMMIT_REF_SLUG
    # Include copying git submodules. #
    GIT_STRATEGY: clone
    GIT_SUBMODULE_STRATEGY: recursive
    GIT_SUBMODULE_UPDATE_FLAGS: --init
    GIT_SUBMODULE_FORCE_HTTPS: "true"
    # Point to the Dockerfile that should be built. #
    DOCKERFILE_PATH: ./.devcontainer/web.Dockerfile
    BUILD_ARGS:
  script:
    # Login to the GitLab registry using the private unique password provided by GitLab. #
    - echo $CI_REGISTRY_PASSWORD | docker login -u $CI_REGISTRY_USER --password-stdin  $CI_REGISTRY
    # Build the Docker image. #
    - docker build --pull -t $FULL_CONTAINER_PATH -f $DOCKERFILE_PATH . $BUILD_ARGS
    # Push the Docker image to the project's container registry. #
    - docker push --quiet $FULL_CONTAINER_PATH
    - docker logout
```

Note that this job is also not specifically Drupal. This can be used for any project with a Dockerfile that needs built and stored in the GitLab container registry.

## Individual Project Jobs

Here is an example of a specific project implementing that build, with some more Drupal details added.

It extends the template file, defining some key variables that need to pass through. Those include the build arguments, which align with the ARGs needed in [the Dockerfile](/2025/devenv-dockerfiles/).

Rules define under what circumstances this build should happen. This is largely a question of efficiency. You could have it build for every change, but building takes time. You might really slow down your processes if you need to wait for a build on every change. Instead, I've tried to define the exact scenarios where I do want to build:

- If the branch is a major release branch, which I can predict because they start with 202 (my release branches are typically in the format YYYY-MM, so this will work until 2030). Those release branches are usually what I am building from in the local development environment, so I will almost always want those to be ready in case I or another developer need a new local build.
- If the branch is dev, staging, or main. Those are going to deploy to other environments, so always need built.
- If a change was made to certain files that impact the building itself, like the Dockerfile or the docker-compose. If I am changing those, it probably means that I am trying to test a change in the build process so I will want to now test the building.
- If a change was made to included packages or to the custom code directories, because I will need that updated image to be accurate for automated regression tests to be able to run. I'll get into regression tests more in a later post.
- The tag is a GitLab Runner that is designed for building Docker images.

```yml
include:
  - project: "ryan-l-robinson/gitlab-ci"
    ref: main
    file: build.yml

## Stages ##
stages:
  - build

build:
  extends: .build_dockerfile
  variables:
    IMAGE_TAG: web
    DOCKERFILE_PATH: ./Dockerfile.Drupal
    BUILD_ARGS: --build-arg BRANCH=$CI_COMMIT_BRANCH --build-arg BASE_URL=$BASE_URL --build-arg TIMESTAMP=$CI_COMMIT_TIMESTAMP
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

That's it!
