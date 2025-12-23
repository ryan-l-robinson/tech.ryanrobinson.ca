---
title: Deployment Overview
date: 2025-08-24T16:54:16.700Z
author: Ryan Robinson
description: "Beginning a series on my Drupal development setup, including Docker containerized deploys, this provides an overview of the environments."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
---

My Drupal development environment has gone through a few iterations over the past 4 years, some of which is chronicled here and on [my GitLab](https://gitlab.com/ryan-l-robinson/drupal-dev-environment). Since I last wrote about it, it went through another big update that includes now fully deploying the web service to a Docker swarm, not just pushing out code updates to each server.

In this post I'll provide an overview of some of what changed, then I'll try to break down more details over some following posts.

## The Environments

### Local Environment

Conceptually this first stage is similar to previous ideas, but it did need a lot of tweaks to get it to work in a way that also worked for the later plans. I'll get into those in a future post. In the meantime for those who haven't followed the previous incarnations, here is the overview of the key features:

- The VS Code devcontainer functionality handles everything. All I need to do is open the folder in VS Code with the devcontainer extensions enabled, then select the option to open in container.
- One Docker container for web (Apache and PHP, the main one we connect to), and one for the database.
- Volumes include the database as well as some key files that you might want to be able to edit from either within the container or directly on the host, mainly the files that help build the devcontainer like the docker-compose files.
- VS Code extensions are defined in the devcontainer.json so that every developer has the same tools installed and we don't have to remember what to set up each time.
- Web container is pulled from the GitLab container registry, saving time on rebuilding.
- DB container is not pulled from the GitLab container registry. This was considered, and a previous version before we got to Docker deploys was doing it. Now we have a better approach, where the script that starts when web is first loaded has an opportunity to load from an old database dump.

### Dev and Staging

In our setup, we have two middle environments: dev and staging. Dev could also be called "proof of concept." That is where we put code when we want to collect feedback from others about an idea before finishing it, or in rare cases when we need to test something that can't be as easily done on the local developer environment, like sending emails. Staging is the last stop before production; it only gets deployed there when we think it is ready for production. After that, it should only need bug fixes or other small and safe tweaks.

They are both set up as Docker Swarms, but only one server ode, and only getting 1 replica of the web container as well as 1 replica of the db container deployed. This is because they are not public and don't get a huge amount of traffic. We do not need redundancy there.

They have volumes for Drupal's public and private folders, on the same server. So dev and staging have almost everything on that one specific server: web containers, db containers, and volumes. The only exception is a volume for database backups, which goes in a network file share that is accessible by all of dev, staging, and production. I'll try to unpack why that is some more later.

There are some differences in dev vs staging in terms of what gets built, e.g. dev has more developer tools enabled, while staging almost exactly replicates the production environment in its settings. For the purposes of this series of posts, they'll be largely treated as the same thing.

### Production

Unlike dev and staging, production is 3 server nodes in a Docker swarm.

The web container is deployed with 3 replicas, which does not necessarily mean one per node, but will overall balance along with any other services that are included.

Public and private files are put in network file shares. This allows that no matter which swarm node happened to get the file uploaded to it, the other nodes will immediately have access to it. There's no risk that somebody uploads a file on server01 and somebody on server02 can't see it. In our previous setup, we had two servers, but a script had to run every 5 minutes on cron to keep them in sync.

The database is on a different dedicated server. This is primarily for performance reasons. While having a database container running alongside on dev and staging is fine, it would likely not be able to keep up with heavy production loads.

## The Techniques

There were a few general techniques that we had to use to get this fully working, which I'll overview here.

### One-Time Scripts

There are some tasks that you want to be able to run on a manual basis on the containers. To set these up, we added extra services that default to 0 replicas deployed, but then a GitLab CI/CD job can launch it. Some of those will be demonstrated in more detail later.

This is the CI/CD job that gets extended for those single-use scripts:

The full version looks complicated mainly because of everything needed to show logs back to the GitLab CI/CD interface, but the core part is really just this: `docker service update --with-registry-auth --replicas 1 --force ${STACK_NAME}_${SERVICE_NAME}`, running the specified service which contains the script we want to run.

### Dockerfile Branching

Dockerfiles can have logic built-in to build a little bit differently depending on the environment. For example, we don't need npm to support Playwright accessibility testing tools on production. That is only needed on local, where we do the building and the testing.

This is done by passing in a variable for environment as part the build:

```yml
build:
  extends: .build_dockerfile
  variables:
    IMAGE_TAG: web
    DOCKERFILE_PATH: ./Dockerfile.Drupal
    BUILD_ARGS: --build-arg BRANCH=$CI_COMMIT_BRANCH --build-arg BASE_URL=$BASE_URL --build-arg TIMESTAMP=$CI_COMMIT_TIMESTAMP
```

That is extending the build_dockerfile job defined elsewhere. That job looks like this:

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

The relevant sections in the Dockerfile look like:

```Dockerfile
ARG BRANCH

RUN if [ "$BRANCH" != "main" ]; then \
apt-get -y --no-install-recommends install \
nodejs \
npm \
;fi
```

### Multiple Docker-Compose

We also found we needed multiple docker-compose files for a couple of unique scenarios, including the distinction of the production network file share volumes as compared to the host machine volumes on dev and staging. These can be merged together in a GitLab CI/CD job.

For example, here's the dev deploy job, with multiple docker-compose:

```yml
dev_deploy:
  stage: deploy
  extends: .deploy_template_swarm
  allow_failure: false
  environment: Development
  variables:
    STACK_NAME: library
    COMPOSE_FILES: docker-compose.yml:docker-compose.local.storage.yml
    LIBWEB_REPLICA_COUNT: 1
    LIBDB_REPLICA_COUNT: 1
  tags:
    - dev
  only:
    refs:
      - dev
```

In addition to the general docker-compose, there's another one for the local storage, telling it save the Drupal public and private files.

The deploy job which that extends looks like this, able to handle parsing the multiple compose files:

```yml
.deploy_template_swarm:
  stage: deploy
  retry:
    max: 2
    when:
      - script_failure
  variables:
    STACK_NAME: ""
    COMPOSE_FILES: docker-compose.yml
  script:
    - echo "$CI_REGISTRY_PASSWORD" | docker login $CI_REGISTRY -u $CI_REGISTRY_USER --password-stdin
    - if [ -z $COMPOSE_FILES ]; then export COMPOSE_FILES=docker-compose.yml; fi
    - >-
      docker --log-level fatal
      compose
      $(printf ' -f %s ' $(echo $COMPOSE_FILES | sed 's/:/ /g'))
      -p $STACK_NAME
      config
      -o docker-compose.rendered.yml
    - sed -i '/^name:.*$/d' docker-compose.rendered.yml
    - docker stack deploy
      -c docker-compose.rendered.yml
      --with-registry-auth
      --detach=false $STACK_NAME
    - docker logout
  artifacts:
    when: always
    paths:
      - docker-compose.rendered.yml
```
