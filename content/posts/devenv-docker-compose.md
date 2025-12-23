---
title: "The Docker Compose Setup"
date: 2025-12-23T14:16:00.000Z
author: Ryan Robinson
description: "Docker Compose for the Drupal setup, including a two-tiered approach for differentiating local."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
draft: true
---
This post is one of several in a series detailing my development environment and CI/CD deployment pipeline. [The code for the developer environment can be seen on my GitLab](https://gitlab.com/ryan-l-robinson/drupal-dev-environment). Other posts in the series can be seen by checking the Drupal Docker series links in the sidebar. I provided [an overview in the first post of the series](/2025/drupal-docker-deploys-overview/).

We now have [a robust Dockerfile for the main web image](/2025/devenv-dockerfiles/), [some scripts to run when it starts up](/2025/devenv-start-script/), and the main web image is being built by CI/CD and stored in the GitLab container registry. There are other services needed, though, and there are some variations needed between local development and the other environments.

## The Two-Tier Setup

There is a docker-compose file that defines services for all environments, saved as docker-compose.yml on the top level of the project, then another docker-compose that only runs on local, saved as docker-compose.yml in the .devcontainer folder alongside other local-specific files. This is done so that there can be some significant differences between the setup for the local developer environment and the other servers it can be deployed to.

## The Primary Docker-compose.yml

### Shared Variables

First, you can define some shared blocks of variables so that they don't need to be repeated in multiple services. In this case, everything is simply passing through the variables that are set from the GitLab CI/CD. Never hardcode sensitive variables like database connection information; store them securely and have them dynamically inserted instead.

```yml
x-mysql-var: &mysql
  MYSQL_DATABASE: ${MYSQL_DATABASE}
  MYSQL_USER: ${MYSQL_USER}
  MYSQL_PASSWORD: ${MYSQL_PASSWORD}
  MYSQL_HOST: ${MYSQL_HOST}
  MYSQL_PORT: ${MYSQL_PORT}

x-config-split: &config-split
  CONFIG_SPLIT_LOCAL: ${CONFIG_SPLIT_LOCAL}
  CONFIG_SPLIT_DEV: ${CONFIG_SPLIT_DEV}
  CONFIG_SPLIT_STAGING: ${CONFIG_SPLIT_STAGING}
  CONFIG_SPLIT_PROD: ${CONFIG_SPLIT_PROD}

x-deploy-env: &deploy-env
  DEPLOY_ENV: ${CI_COMMIT_REF_NAME:-dev}
```

### The Web Service

The primary web service is the first defined. Some notes here include:

- The image is fetched from the GitLab repository. I haven't written up about the Docker image being built yet, but that is done by CI/CD and the image is stored in the container registry, where this docker-compose can then grab it.

```yml
services:
  web:
    image: ${CI_REGISTRY_IMAGE:-registry.gitlab.com/ryan-l-robinson/drupal-dev-environment/web}:${CI_COMMIT_REF_NAME:-dev}
    restart: unless-stopped
    environment:
      <<: [*mysql, *config-split, *deploy-env]
    volumes:
      - private:/opt/drupal/private
      - public:/opt/drupal/web/sites/default/files
    networks:
      - default
      - proxy
    deploy:
      mode: replicated
      replicas: ${WEB_REPLICA_COUNT:-1}
      labels:
        - "traefik.enable=true"
        - "traefik.http.routers.web.rule=Host(`${HOST:-drupal.devenv}`)"
        - "traefik.http.routers.web.entrypoints=https"
        - "traefik.http.routers.web.service=web@docker"
        - "traefik.http.routers.web.tls=true"
        - "traefik.http.services.web.loadbalancer.passhostheader=true"
        - "traefik.http.services.web.loadbalancer.server.port=443"
        - "traefik.http.services.web.loadbalancer.server.scheme=https"
      update_config:
        parallelism: 1
        delay: 30s
        order: start-first
    healthcheck:
      test: curl --fail http://localhost/health || exit 1
      retries: 9
      timeout: 10s
      start_period: 15s
```
