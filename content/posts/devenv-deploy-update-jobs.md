---
title: Deploying Containers
date: 2025-12-23T15:27:16.700Z
author: Ryan Robinson
description: "Using GitLab CI/CD to deploy new containers with Docker swarm, and run the Drupal database update script."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
  - Linux
draft: true
---

This post is one of several in a series detailing my development environment and CI/CD deployment pipeline. [The code for the developer environment can be seen on my GitLab](https://gitlab.com/ryan-l-robinson/drupal-dev-environment). Other posts in the series can be seen by checking the Drupal Docker series links in the sidebar. I provided [an overview in the first post of the series](/2025/drupal-docker-deploys-overview/).

In this post, I will look at the core of the process for deploying updates to a Docker swarm server.

We're starting to reach some of the parts of this series that I cannot give full code that I know works in other environments, because I do not have personally have my own Docker swarm servers to test things with. This component is based on what works with some of my work infrastructure. On a related note, I have structured the demo project so that jobs I don't actually have the environment to run are in the swarm.gitlab-ci.yml, as opposed to the main .gitlab-ci.yml which will actually run when I make new changes.

## Deploy Job

First up, we need to deploy the updated service to the swarm.

### General Deployment Template

The core function to be able to deploy a stack to a Drupal swarm is `docker stack deploy` but this gets more complicated because of another requirement: sometimes there is another docker-compose required to define differences for the other environments. I don't have those in my example, but they might be things like designating where to find the volumes, since the file structure on all environments is not always going to be the same. Because of that, it needs to first merge together more than one docker-compose file, before it deploys the stack of that combined file. It also will provide that combined file as an artifact, in case it's helpful for debugging issues.

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

### Project Implementation for Deployment

To implement that in a particular project, it would look something like:

```yml
prod_deploy:
  stage: deploy
  extends: .deploy_template_swarm
  allow_failure: false
  environment: Production
  variables:
    STACK_NAME: library
    COMPOSE_FILES: docker-compose.yml:docker-compose.local.storage.yml
    WEB_REPLICA_COUNT: 3
    DB_REPLICA_COUNT: 1
  tags:
    - prod
  only:
    refs:
      - main
```

This extends the template job. It requires combining two docker-compose files. It also passes through a couple of variables that we saw back in [the docker-compose file](/2025/devenv-docker-compose/) to declare how many of each service we want to deploy to the swarm. This one runs on the prod server's tag, and only when merges are made into the main branch.

## Database Update Job

Next, with Drupal specifically, we need to update the database to be in line with the new code. Clearing caches is also an essential step in Drupal deployments.

### General Single-Use Template

To do this, I have a general "single use" job. At its simplest, it runs a script. Like the deployment, though, it gets more complicated, in this case because I wanted to be able to see the logs of that script running so that we know if there are any problems. That is what makes this a much longer script.

```yml
## This is for single-use services, to run a specific script one time, before or after deployment. ##
.docker_single_use:
  variables:
    STACK_NAME: ""
    SERVICE_NAME: ""
    SERVICE_UP_WAIT_REPEAT: 12
    SERVICE_UP_WAIT_TIME: 5
    SERVICE_CREATE_WAIT_REPEAT: 60
    SERVICE_CREATE_WAIT_TIME: 5
  script:
    - echo "$CI_DEPLOY_PASSWORD" | docker login -u $CI_DEPLOY_USER --password-stdin $CI_REGISTRY
    - DATE="$(date --iso-8601=seconds)"
    - docker service update --with-registry-auth --replicas 1 --force ${STACK_NAME}_${SERVICE_NAME} --update-monitor 1s
    - |
      EXIT_VALUE=0
      SERVICE_UP_WAIT_CHECK=0
      while [ -z "$(docker service ps --format 'json' ${STACK_NAME}_${SERVICE_NAME})" ]; do
        SERVICE_UP_WAIT_CHECK=$((SERVICE_UP_WAIT_CHECK+1))
        if [ "${SERVICE_UP_WAIT_CHECK}" -eq "${SERVICE_UP_WAIT_REPEAT}"]; then
          echo Service failed to be created within $((SERVICE_UP_WAIT_REPEAT * SERVICE_UP_WAIT_TIME)) seconds.
          exit 1
        else
          echo Service ${STACK_NAME}_${SERVICE_NAME} has not been created yet. Attempt ${SERVICE_UP_WAIT_CHECK}/${SERVICE_UP_WAIT_REPEAT}
          sleep ${SERVICE_UP_WAIT_TIME}
        fi
      done
      SERVICE_CREATE_WAIT_CHECK=0

      for task in $(docker service ps -q ${STACK_NAME}_${SERVICE_NAME}); do
        # Get the CreatedAt of the current task for comparison
        created_at=$(docker inspect --format '{{.CreatedAt}}' $task)

        # Check if this task is more recent than the current most recent task
        if [[ -z "$most_recent_created_at" || "$created_at" > "$most_recent_created_at" ]]; then
          LATEST_TASK=$(docker inspect --format '{{.ID}}' $task)
          most_recent_created_at="$created_at"
        fi
      done

      while true; do
        NEW_DATE="$(date --iso-8601=seconds)"
        docker service logs --since="${DATE}" ${STACK_NAME}_${SERVICE_NAME}
        DATE="${NEW_DATE}"
        DOCKER_STATUS=$(docker inspect --format '{{.Status.State}}' $LATEST_TASK)
        echo "Latest task status is ${DOCKER_STATUS}"
        if [ "${DOCKER_STATUS}" = "failed" ]; then
          EXIT_VALUE=1
          break
        elif [ "${DOCKER_STATUS}" = "complete" ]; then
          break
        elif [ "${DOCKER_STATUS}" = "running" ]; then
          sleep ${SERVICE_CREATE_WAIT_TIME}
        else
          SERVICE_CREATE_WAIT_CHECK=$((SERVICE_CREATE_WAIT_CHECK+1))
          if [ "${SERVICE_CREATE_WAIT_CHECK}" -eq "${SERVICE_CREATE_WAIT_REPEAT}" ]; then
            echo Service failed to start within $((SERVICE_CREATE_WAIT_REPEAT * SERVICE_CREATE_WAIT_TIME)) seconds.
            EXIT_VALUE=1
            break
          fi
          sleep ${SERVICE_CREATE_WAIT_TIME}
        fi
      done
      docker service logs --since="${DATE}" ${STACK_NAME}_${SERVICE_NAME}
    - docker logout
    - exit $EXIT_VALUE
```

This allows it to loop on the service, returning its status and any logs that have been added since the last check.

### Project Implementation for Database Update

To run this now on a specific project, you need a few things.

First is the script that it is going to run, in a separate file under the scripts folder. As with [the start script](/2025/devenv-start-script/), it will first wait to confirm that it is able to connect to the database, returning an error if it didn't work after enough tries. Then it will use drush commands to import the latest configuration - twice, because if config_split is involved it might only apply the split in the first run and then change the split configurations on the second - then run any updates needed to the database and clear the caches.

```bash
#!/bin/bash

wait_for_db() {
  local db_retries=10
  local db_wait_time=5
  local db_attempt=1

  source /opt/drupal/scripts/settings-file.sh

  while [ $db_attempt -le $db_retries ]; do
    if drush sqlq "SELECT 1" > /dev/null 2>&1; then
      return 0
    else
      echo "Database is not ready yet. Attempt $db_attempt/$db_retries. Waiting for $db_wait_time seconds..."
      sleep $db_wait_time
      db_attempt=$((db_attempt + 1))
      db_wait_time=$((db_wait_time * 2))
    fi
  done

  return 1
}

if wait_for_db; then

  set -e
  drush config-import -y
  # Run again to catch extra configuration in the config_split
  drush config-import -y

  drush updb -y
  drush cr

else
  echo "Database never came up."
  exit 1
fi
sleep 30
```

Next is the service declared in the docker-compose. This runs the main web image, but replacing the command with the update script we just wrote. It will normally not replicate at all in a general deploy, because we do not want this to be able to run until after we know the new web service is fully in place.

```yml
  db_update:
    image: ${CI_REGISTRY_IMAGE:-registry.gitlab.com/ryan-l-robinson/drupal-dev-environment/web}:${CI_COMMIT_REF_NAME:-main}
    command: /opt/drupal/scripts/update.sh
    environment:
      <<: [*mysql, *config-split, *deploy-env]
    volumes:
      - private:/opt/drupal/private
      - public:/opt/drupal/web/sites/default/files
    deploy:
      mode: replicated
      replicas: 0
      restart_policy:
        condition: on-failure
        max_attempts: 1
```

Finally is the GitLab CI/CD job that will extend the single-use template and specify which service (running which script) it needs to run. This will start up that service just once, after we know the other deployment job is done, run its script and then shut down again.

```yml
prod_update:
  stage: deploy
  extends: .docker_single_use
  environment: Production
  variables:
    STACK_NAME: library
    SERVICE_NAME: db_update
  tags:
    - prod
  only:
    refs:
      - dev
  needs: [dev_deploy]
```

I know that seems like it is much more complicated than "run a script" sounds like it should be, but it's designed to be flexible for other services running any scripts as well as user-friendly with the logs visible.
