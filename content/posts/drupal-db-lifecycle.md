---
title: Database Lifecycle
date: 2026-01-13T17:11:16.700Z
author: Ryan Robinson
description: "I developed a semi-automated method to keep databases roughly in sync across environments."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
  - Linux
---

This series has now covered all the core functionality of having a local development environment and deploying updates, and most elements of testing (phpunit regression tests is coming later, as there are some things I have not quite solved in the CI/CD part yet).

In this post, I will get into a database lifecycle strategy, used to help ensure that my developer environment, dev server, and staging server are roughly reflecting realistic content, even if not exactly up-to-date. This is done with a series of the "single use" jobs that I mentioned in [the deployment post](/2025/devenv-deploy-update-jobs/).

Unlike most else from this series, none of this is in my example Drupal developer environment project, since I don't have actual servers personally to test any of it against. I am generalizing from my work context.

## Dump a (Production) Database

The first step is to dump the production database, so that I will have it ready to copy to the other environments. In the docker-compose file of the project, it includes this service:

```yml
  db_dump:
    image: ${CI_REGISTRY_IMAGE:-[YOUR REPO HERE]/web:${CI_COMMIT_REF_NAME:-main}
    command: /opt/drupal/scripts/dump-db.sh
    environment:
      <<: [*mysql, *config-split, *deploy-env]
    volumes:
      - private:/opt/drupal/private
      - public:/opt/drupal/web/sites/default/files
      - backups:/opt/drupal/backups
    deploy:
      mode: replicated
      replicas: 0
      restart_policy:
        condition: on-failure
        max_attempts: 1
```

This tells it to run once, using the main project image going through the script dump-db, and then stop. There are the usual two volumes for Drupal private and public files, plus one more for the backup that is going to be generated. This is mapped elsewhere to be a network file share, which will be necessary for the other servers to also be able to grab it easily when it comes time to import it.

The script that it runs is simple:

```bash
#!/bin/bash

source /opt/drupal/scripts/settings-file.sh

drush sql:dump --result-file="/opt/drupal/backups/$(date '+%Y-%m-%d_%H-%M-%S')_prod.sql"
sleep 30
```

The settings-file I have already covered elsewhere; that is used for importing the database connection that varies by environment. Otherwise, it's making use of a drush command to dump a file to backups using today's date to help sort it and differentiate it from previous backups. The 30 second sleep is to make sure this script runs long enough to report back to the single-use job CI/CD script.

Finally, to run that, the project's CI/CD job might look like this:

```yml
prod_db_dump:
  stage: deploy
  allow_failure: false
  extends: .docker_single_use
  environment: Production
  variables:
    STACK_NAME: drupal
    SERVICE_NAME: db_dump
  environment:
    name: Production
  tags:
    - prod
  only:
    refs:
      - main
  when: manual
```

## Download the File for Local

Another job will show you the newest database backup file from the shared storage, allowing you to download it as an artifact. This lets you keep it as a backup in case anything goes wrong, like just before a deployment, as well as to update the content on the local developer environment.

I'm not going to repeat the docker-compose and the GitLab CI, since they're basically the same, but here's the script:

```bash
#!/bin/bash

BACKUP_DIR="$1"
BACKUP_FILE="$(ls -1r "${BACKUP_DIR}" | head -n 1)"
BACKUP_FILE_FULL_PATH="$(readlink -f ${BACKUP_DIR}/${BACKUP_FILE})"
ARTIFACT_DIR="$2"

echo "Exporting ${BACKUP_FILE_FULL_PATH} as an artifact..."
cp -v "${BACKUP_FILE_FULL_PATH}" "${ARTIFACT_DIR}"
sleep 30
```

## Import to Dev or Staging

Finally, another job with a similar setup will allow running on dev or staging to import that database there. Again I'll share just the script:

```bash
#!/bin/bash

source /opt/drupal/scripts/settings-file.sh

BACKUP_FILE="/opt/drupal/backups/$(ls -1r /opt/drupal/backups | head -n 1)"

if [ "${BACKUP_FILE}" == "" ]; then
    echo "No backups found"
    exit 1
else
    echo "Applying SQL from ${BACKUP_FILE}..."

    $(drush sql:connect) < ${BACKUP_FILE}

    drush cr

    # Update database if files are now ahead of backup.
    drush updb -y

    # Update config twice to ensure any config_split differences
    drush config-import -y
    drush config-import -y

    # Activate and set the distinct local password on the admin user.
    drush user:unblock $DRUPAL_USER_NAME
    drush upwd $DRUPAL_USER_NAME $DRUPAL_USER_PASSWORD

    drush cr
fi
```

This imports the latest backup file that was found. It then does a few more things:

- clears caches
- completes any due database updates, which could be necessary if the code for dev/staging is already ahead of the backup
- imports configuration twice (to account for config_split differences between environments), again to include if the code/config is already ahead of the backup on these other environments
- unblocks the Drupal super-admin that can be used on dev or staging unlike production
- changes the password on that user to the environment-specific one that is stored in GitLab
- clears caches again at the end
