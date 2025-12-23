---
title: "Drupal Docker: Start Scripts"
date: 2026-01-29T01:46:00.000Z
author: Ryan Robinson
description: "The start script for a Drupal Docker setup includes connecting to the database, setting up key files with variables, and running Apache."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
  - Linux
draft: true
---

When the main web container starts up, it runs the script specified at the end of its Dockerfile, which is simply called start.sh in my setup.

## Start.sh

The start script itself is relatively simple, but it is calling on a few other scripts which I will get into more below. The basic idea is that it will wait for the database container to be available. Once it is, it will check if the database is already installed. If it's not, the database can be rebuilt (more in another file). Otherwise, simply prepare the phpunit file and then start up Apache.

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

## Settings-file.sh

The settings file script will fill in the Drupal settings file, using variables that are passed through from the GitLab variables.

```bash
#!/bin/bash

# Update template from variables
if [ ! -f /opt/drupal/web/sites/default/settings.php ]; then
  echo "export DEPLOYMENT_ID=${TIMESTAMP:0:10}" >> ~/.bashrc
  source ~/.bashrc
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

1. If it already exists, don't do anything new. This keeps it safe when deploying to environments with databases saved as volumes which should not be wiped out.
2. If it doesn't exist, but there is a backup file in the backups folder, start with that.
3. If it doesn't exist and there is no backup file, build a new site from the configuration. Normally in this setup now, it shouldn't ever need to get this far, but if it does, that's still a usable site, just with a lot less content to be a good representation of reality.

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

## Phpunit-file.sh

The phpunit file I may also get into more in a future post, but it is a lot like the settings file, simply substituting variables into the file.

```bash
#!/bin/bash

envsubst '${MYSQL_DATABASE} ${MYSQL_USER} ${MYSQL_PASSWORD} ${MYSQL_HOST} ${MYSQL_PORT}' < /opt/drupal/phpunit.xml.tmpl > /opt/drupal/phpunit.xml
```
