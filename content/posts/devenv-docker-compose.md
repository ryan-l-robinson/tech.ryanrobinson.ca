---
title: "The Docker Compose Setup"
date: 2025-12-23T14:16:00.000Z
author: Ryan Robinson
description: "Docker Compose for the Drupal setup, including a two-tiered approach for differentiating local."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
---
This post is one of several in a series detailing my development environment and CI/CD deployment pipeline. [The code for the developer environment can be seen on my GitLab](https://gitlab.com/ryan-l-robinson/drupal-dev-environment). Other posts in the series can be seen by checking the Drupal Docker series links in the sidebar. I provided [an overview in the first post of the series](/2025/drupal-docker-deploys-overview/).

We now have [a robust Dockerfile for the main web image](/2025/devenv-dockerfiles/), [some scripts to run when it starts up](/2025/devenv-start-script/), and [that main web image is being built by CI/CD and stored in the GitLab container registry](/2025/devenv-build-image-ci/). Now we can start to put some of these pieces together, using docker-compose to define all the required services and how they relate to each other.

## The Two-Tier Setup

There is a docker-compose file that defines services for all environments, saved as docker-compose.yml on the top level of the project, then another docker-compose that only runs on local, saved as docker-compose.yml in the .devcontainer folder alongside other local-specific files. This is done so that there can be some important differences between the setup for the local developer environment and the other servers it can be deployed to.

## The Primary Docker-compose.yml

### Shared Variables

First, you can define some shared blocks of variables so that they don't need to be repeated in multiple services. In this case, everything is simply passing through the variables that are set from [the GitLab CI/CD Variables](https://docs.gitlab.com/ci/variables/) for the project. Never hardcode sensitive variables like database connection information; store them securely and have them dynamically inserted instead.

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

- The image is fetched from the GitLab repository. That's the one that was built with the CI/CD job in the last post.
- Environment variables include those defined at the start of the file.
- There are volumes for Drupal's private files and Drupal's public files. Those files need to be persistent when deploying updates to other environments that already have content on them.
- The external proxy network has to do with adding traefik support, which we also see in the deploy labels. I am not going to get deeply into that any time soon, as that was largely done by the server management team and I haven't fully wrapped my head around traefik yet.
- When it deploys, it will replicate a set number of times. This varies depending on the environment, with it falling back to 1. In our case we are generally deploying 3 copies to production across a Docker swarm to help with load balancing, but only 1 on local developer environment and the dev server and the staging server where the demand is much lower and there's no concern about downtime during the deployment.
- It will only deploy 1 at a time, starting the updated replacement before deleting the old one, and wait 30 seconds between each one. There is some tradeoff here. If it goes too fast, the updated code image might be in place faster but not successfully running Apache quite yet, in which case you could get a gap where there is no functioning copy. It is also true that some (not all) updates will cause errors until the Drupal update script is done. There's only one database, which can't be updated until all the replicas are in place. Going too fast means almost certain short downtime for every deployment. Going too slow could mean longer than necessary downtime for those occasional deployments that need the database update to resolve serious issues.
- There is a health check. Traefik won't serve a container that says it isn't healthy. In this case, I'm using [a Drupal health_check module](https://www.drupal.org/project/health_check) that generates a page at /health, so this check will confirm if that page is available. If it's not, Drupal isn't functioning yet, so it should prioritize the other containers.

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

### The Database Service

The database is the other absolutely essential component to have a functioning Drupal site. Some notes:

- In this demonstration, I'm using the latest mariadb image. You might want a different one to match a different production database version, to ensure you're giving an adequate test.
- It gets the same environment variables as the web image to define the MySQL credentials, as well as setting a random root password which isn't truly necessary.
- It similarly deploys replicated with a variable and a fallback of 1. As with web, we are deploying one for local, dev, and staging. Unlike web, we are deploying 0 for production, because that uses a dedicated server instead of a containerized database.
- The volume is for the database data which stays persistent through updates.

```yml
  db:
    image: mariadb:latest
    restart: unless-stopped
    environment:
      <<: [*mysql]
      MYSQL_RANDOM_ROOT_PASSWORD: "yes"
      MYSQL_ONETIME_PASSWORD: "yes"
    deploy:
      mode: replicated
      replicas: ${DB_REPLICA_COUNT:-1}
    volumes:
      - db:/var/lib/mysql
```

### Single Run Services

The rest are a variety of single use jobs. These all will spin up the service when requested by the GitLab CI/CD job, run their script once, and then shut down again. I'll cover most of these at some point in the relevant stage, but at a high level, there's one really important one for the database update after a deployment, some related to testing, and some related to the database lifecycle processes of backing up production data and importing it in other environments.

## The Local Dev Environment Docker-Compose

The second docker-compose file sets up some things differently for the local developer environment. Some notes on differences:

- The MySQL credentials are hardcoded. It's fine here because this will only impact your local developer environment, never impacting data that anybody else has access to.
- There's a port mapping to 443 (https) so that it can be browseable from the host machine.
- There are more volumes of files that are persistent with the host machine. Most of these are the ones that are involved in building it, so that I can make a change inside the running environment, close it, then rebuild it again with the change still in place. Otherwise I would have to commit it to GitLab, close it, pull the change back down to the host machine version, then could rebuild.
- The proxy network is marked as not external. The local dev environment doesn't have the traefik requirements, at least not yet (I have tried to use it locally, to facilitate better certificates and better domain names for browsing, but didn't get far).

That gives us our general array of services. Next up will be some more instructions for our local developer environment, including how it knows to combine both docker-compose files.
