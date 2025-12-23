---
title: "Start Scripts"
date: 2025-12-23T11:22:00.000Z
author: Ryan Robinson
description: "Key scripts for a Drupal Docker setup includes connecting to and building the database, setting up key files with variables, and running Apache."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
  - Linux
---

This post is one of several in a series detailing my development environment and CI/CD deployment pipeline for a Drupal site. [The code for the developer environment can be seen on my GitLab](https://gitlab.com/ryan-l-robinson/drupal-dev-environment). Other posts in the series can be seen by checking the Drupal Docker series links in the sidebar. I provided an overview in [the first post of the series](/2025/drupal-docker-deploys-overview/).

In [the last post](/2025/devenv-dockerfiles/), I created the web Dockerfile. At the end of that file it specifies what script to run when it starts up. This is simply called start.sh in my setup, and it's that script that I will now pick up from for this post.

## Start.sh

The start script itself is relatively simple, but it is calling on a few other scripts. The idea here is that it will wait for the database container to be available. Once it is, it will check if the database is already installed. If it's not, the database can be rebuilt. Finally, prepare the phpunit.xml file and then start up Apache.

```bash
#!/bin/bash

# Wait for the database to come up
wait_for_db() {
  local retries=10
  local wait_time=5
  local attempt=1

  source /opt/drupal/scripts/settings-file.sh

  while [ $attempt -le $retries ]; do
    if drush sqlq "SELECT 1" > /dev/null 2>&1; then
      return 0
    else
      echo "Database is not ready yet. Attempt $attempt/$retries. Waiting for $wait_time seconds..."
      sleep $wait_time
      attempt=$((attempt + 1))
      wait_time=$((wait_time * 2))
    fi
  done

  return 1
}

if wait_for_db; then
  if $(drush sqlq "SHOW TABLES LIKE 'users'" | grep -q users); then
    echo Drupal has already been initialized.
  else
    source /opt/drupal/scripts/rebuild-db.sh
  fi

  source /opt/drupal/scripts/phpunit-file.sh
  apache2-foreground

else
  echo "Database never came up."
  exit 1
fi
```

Now, let's get into some of those other files that are referenced by the start file, as well as potentially referenced in other workflows as well.

## Settings-file.sh

The settings file script will fill in the Drupal settings file, using variables that are passed through from the GitLab variables (most of them) or from the GitLab build job as an argument (the timestamp that becomes the deployment ID). This is done using the envsubst command to generate a new file with the correct name in the correct location by replacing those variables in a template file. This is necessary to allow that each environment has different variables like database connection information.

```bash
#!/bin/bash

# Update template from variables.
if [ ! -f /opt/drupal/web/sites/default/settings.php ]; then
  # First 10 digits are just the date.
  echo "export DEPLOYMENT_ID=${TIMESTAMP:0:10}" >> ~/.bashrc
  source ~/.bashrc
  # Create the new file from the template with variable replacement.
  envsubst '${DEPLOYMENT_ID} ${MYSQL_DATABASE} ${MYSQL_USER} ${MYSQL_PASSWORD} ${MYSQL_HOST} ${MYSQL_PORT} ${CONFIG_SPLIT_LOCAL} ${CONFIG_SPLIT_DEV} ${CONFIG_SPLIT_STAGING} ${CONFIG_SPLIT_PROD}' < /opt/drupal/settings.php.tmpl > /opt/drupal/web/sites/default/settings.php
fi

# Dev tools
if [ "$DEPLOY_ENV" != "main" ] && [ "$DEPLOY_ENV" != "staging" ]; then
  cp -f /opt/drupal/.devcontainer/settings.dev.php /opt/drupal/web/sites/default/settings.dev.php
  cp -f /opt/drupal/.devcontainer/local_dev.services.yml /opt/drupal/web/sites/local_dev.services.yml
fi
```

## Rebuild-db.sh

I will get into the bigger picture of this more in a future post about the whole database lifecycle, but for now, here's the script. There is a hierarchy of options for what to do in terms of building the database:

1. If it already exists, don't do anything new. This keeps it safe when deploying to environments with databases already existing which should not be wiped out.
2. If it doesn't exist, but there is a backup file in the backups folder, start by importing that.
3. If it doesn't exist and there is no backup file, build a new site from the configuration. Normally in this setup now, it shouldn't ever need to get this far, but if it does, that's still a usable site, just with a lot less representative content.

```bash
#!/bin/bash

# Find the newest .sql backup file
newest_file=$(ls -t "/opt/drupal/backups"/*.sql 2>/dev/null | head -n 1)

if [ -z "$newest_file" ]; then
  echo "No .sql backup files found. Starting a new Drupal install."

  # Import config
  drush site-install --existing-config

  # Run again to catch extra configuration in the config_split
  drush config-import -y

  # Add admin user
  drush user:create $DRUPAL_USER_NAME --mail="$DRUPAL_USER_EMAIL" --password="$DRUPAL_USER_PASSWORD"
  drush user:role:add "administrator" $DRUPAL_USER_NAME

else
  echo "The newest backup .sql file is: $newest_file. Installing now."
  source /opt/drupal/scripts/import-db.sh $newest_file

fi

# Rebuild node access caches
drush php-eval 'node_access_rebuild();'
drush cr
```

## Import-db.sh

The rebuild-db.sh file also referenced import-db.sh, which handles the scenario when importing from a database backup file. It then also sets the Drupal user based on the variables, and imports any configuration to incorporate any changes since the backup. This gets used in the initial local dev environment build. It also gets used to update the database on dev and staging servers so they can continue to have relatively accurate content.

```bash
#!/bin/bash

source /opt/drupal/scripts/settings-file.sh

BACKUP_FILE="/opt/drupal/backups/$(ls -1r /opt/drupal/backups | head -n 1)"

if [ "${BACKUP_FILE}" == "" ]; then
    echo "No backups found"
    exit 1
else
    echo "Applying SQL from ${BACKUP_FILE}..."

    drush sql-cli < "${BACKUP_FILE}"

    drush cr

    # Activate and set the distinct local password on the admin user.
    drush user:unblock $DRUPAL_USER_NAME
    drush upwd $DRUPAL_USER_NAME $DRUPAL_USER_PASSWORD

    # Update config twice to ensure any config_split differences
    drush config-import -y
    drush config-import -y

    drush cr
fi
```

## Phpunit-file.sh

The phpunit file I will also get into more in a future post, but it is a lot like the settings file, simply substituting variables into the file.

```bash
#!/bin/bash

envsubst '${MYSQL_DATABASE} ${MYSQL_USER} ${MYSQL_PASSWORD} ${MYSQL_HOST} ${MYSQL_PORT}' < /opt/drupal/phpunit.xml.tmpl > /opt/drupal/phpunit.xml
```
