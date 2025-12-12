---
title: "Drupal Docker: The First-Run Script"
date: 2022-06-29T01:46:00.000Z
author: Ryan Robinson
description: "A first-run script for a Drupal Docker image."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
---

This continues a mini-series describing how I set up a Drupal development environment using [Docker Desktop and the VS Code devcontainer functionality](/tags/drupal-docker/). The full code is available in [my GitHub](https://github.com/ryan-l-robinson/Drupal-Devcontainer).

This post will look at the configuration script that runs on the initial creation of the containers. It will handle the majority of the Drupal-specific functionality, roughly equivalent to [the GitPod.yml file](/posts/2022/drupal-gitpod-container-2-gitpod-yml/) of [the GitPod series](/tags/gitpod-drupal/).

This is a bash script that runs in the container once it is first built, so start by defining it as a bash script:

```bash
#!/bin/bash
```

Enforce permissions on the SSH keys, needed for the git SSH connection to work:

```bash
sudo chown -R drupal:drupal /home/drupal/.ssh
chmod -R 700 ~/.ssh
```

Provide ownership of entire web root to drupal user:

```bash
sudo chown -R drupal:apache /var/www/html
```

Move to codebase:

```bash
cd /var/www/html/local.drupal.com
```

Add local domain to hosts file (useful for tools like pa11y being able to browse the site):

```bash
if [ ! -z $(grep "127.0.0.1 local.drupal.com" /etc/hosts) ]; then
  echo "127.0.0.1 local.drupal.com" >> /etc/hosts
fi
```

Create database and grant all privileges to drupal user. In this case I'm putting it in the script explicitly because I want to share an out-of-the-box functional system. If you're going to do this with a real site, make sure you aren't using the same password on servers that anybody else can access, unlike Docker on your computer. Also it should be a stronger password than "root."

```bash
mysql --host="db" -e "CREATE DATABASE IF NOT EXISTS drupal;" -u root --password="root"
mysql --host="db" -e "GRANT ALL PRIVILEGES ON drupal.* TO 'drupal'@'%';" -u root --password="root"
```

Install site's contributed code base from composer:

```bash
composer install
```

Add drush alias to PATH for easier use:

```bash
echo "alias drush=\"/var/www/html/local.drupal.com/vendor/drush/drush/drush\"" >> ~/.bashrc
```

Copy the Drupal configuration files:

```bash
if [[ -f ./.devcontainer/conf/drupal.settings.php ]]
then
  sudo cp .devcontainer/conf/drupal.settings.php web/sites/default/settings.php
fi
if [[ -f ./.devcontainer/conf/local.services.yml ]]
then
  sudo cp .devcontainer/conf/local.services.yml web/sites/local.services.yml
fi
if [[ ! -d private ]]
then
  mkdir private
  chmod 755 private
fi
if [[ ! -d sync/config ]]
then
  mkdir -p sync/config
fi
```

Import the existing configuration. Note that there is a drush command to build a new site from the existing configuration, but it did not work reliably in this context.

```bash
vendor/drush/drush/drush site-install -y minimal
vendor/drush/drush/drush cset -y system.site uuid "3d9878de-3355-4510-af4d-575deb24055f"
vendor/drush/drush/drush config-import -y
```

Flush generated images, which helps if configuration changes since last time including changes to the media formats. This isn't really necessary when you aren't also using [the content_sync module](https://drupal.org/project/content_sync), which I no longer am in this demo, since without that there are no images to regenerate. But I kept the image flush anyway to demonstrate how that works.

```bash
vendor/drush/drush/drush image-flush --all
```

Sets admin password. The same note applies as with the MySQL passwords above.

```bash
vendor/drush/drush/drush user:password admin "ZNB*ufm1tyz4rwc@yzk"
```

Rebuild the node access caches. This isn't a huge issue, but does save clearing a warning that appears in Drupal.

```bash
vendor/drush/drush/drush php-eval 'node_access_rebuild();'
```

Set the [environment indicator](https://www.drupal.org/project/environment_indicator). This is a Drupal module that shows the site toolbar and favicon with different colours. It's very helpful to visually cue your brain when you're in Docker vs dev vs staging vs production. For this to work, you'll have to include the environment indicator module in your composer.json file.

```bash
vendor/drush/drush/drush cset -y environment_indicator.indicator name "Local Docker"
vendor/drush/drush/drush cset -y environment_indicator.indicator fg_color "#ffffff"
vendor/drush/drush/drush cset -y environment_indicator.indicator bg_color "#000000"
```

Finally, the all-important clearing of the caches:

```bash
vendor/drush/drush/drush cr
```
