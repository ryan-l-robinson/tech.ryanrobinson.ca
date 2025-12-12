---
title: "Drupal Dev Environment Updated: The Dockerfiles"
date: 2024-03-25T00:16:00.000Z
author: Ryan Robinson
description: "An updated version of the Dockerfiles for a Drupal dev environment."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
---

A while ago I wrote about building a [Docker Desktop dev environment for Drupal](/tags/drupal-docker/). It was built on Oracle Linux, a requirement in that context, with three images for Apache, PHP, and MySQL. But there were some significant problems with it, first and foremost that it was very slow. So, this is another version, now based on one of the official Drupal images instead of the Oracle Linux ones. [It can be seen on my GitHub](https://github.com/ryan-l-robinson/Drupal-Devcontainer).

## The Web Image

This new setup has both Apache and PHP in the same container, is about 40% of the total image size compared to the previous one, and runs much faster, probably because Apache and PHP are in the same container instead of having to communicate across the network. Let's start with the basic start to the image and its shell.

```Dockerfile
FROM drupal:php8.2-apache
USER root
SHELL ["/bin/bash", "-c"]
```

Install the extra useful packages - some of the most essential ones are already included in the Drupal image - and PHP development settings. The file copied from the PHP folder covers some of the XDebug configuration settings.

```Dockerfile
# Install needed repositories and general packages, and put the php.ini in place
RUN apt-get update -y \
    && apt-get install -y wget git zip which sudo vim locales default-mysql-client docker nodejs npm \
    && apt-get upgrade -y \
    && mv /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini

# Install PHP extensions, using PECL
RUN pecl channel-update pecl.php.net \
    && pecl install apcu xdebug uploadprogress \
    && docker-php-ext-enable apcu \
    && echo "apc.enable_cli=1" >> /usr/local/etc/php/conf.d/docker-php-ext-apcu.ini \
    && docker-php-ext-enable xdebug \
    && touch /var/log/xdebug.log \
    && chown www-data:www-data /var/log/xdebug.log \
    && docker-php-ext-enable uploadprogress
COPY /php /usr/local/etc/php/conf.d
```

Let the default www-data user have sudo permission without passwords, which would not be the way to go in production but in a development environment is pretty useful in case you ever need to do things like install a new package without rebuilding the whole environment.

```Dockerfile
# Add www-data user to sudo group, and allow those users to sudo without password
RUN usermod -a -G sudo www-data \
    && usermod -d /user/www-data www-data \
    && mkdir -p /user/www-data/.vscode-server \
    && chown -R www-data:www-data /user/www-data \
    && mkdir -p /user/www-data/.ssh \
    && chown -R www-data:www-data /user/www-data/.ssh \
    && chmod 700 -R /user/www-data/.ssh \
    && ssh-keyscan -t rsa gitlab.com >> /user/www-data/.ssh/known_hosts \
    && sed -i "s/%sudo	ALL=(ALL:ALL) ALL/%sudo	ALL=(ALL)	NOPASSWD: ALL/g" /etc/sudoers
```

Fix a locale error that shows up once Apache is running, by specifying the locale it is running in. In my case, that's Canada, with Canadian English.

```Dockerfile
# Fixes locale errors, must happen before Apache. This is using my locale of Canada
RUN echo "LC_ALL=en_CA.UTF-8" >> /etc/environment \
    && echo "en_CA.UTF-8 UTF-8" >> /etc/locale.gen \
    && echo "LANG=en_CA.UTF-8" > /etc/locale.conf \
    && locale-gen en_CA.UTF-8
```

Add a self-signed certificate for the Apache configuration, so that we'll be able to browse the site locally with HTTPS.

```Dockerfile
# Apache configuration, including SSL certificates and logs
COPY /apache /etc/apache2
RUN a2enmod ssl \
    && mkdir -p /etc/apache2/certs \
    && openssl req -batch -newkey rsa:4096 -nodes -sha256 -keyout /etc/apache2/certs/example.com.key -x509 -days 3650 -out /etc/apache2/certs/example.com.crt -config /etc/apache2/certs/openssl-config.txt \
    && chown -R root:www-data /etc/apache2 \
    && chmod 770 -R /etc/apache2/certs
```

Increase some of the default resource limits for PHP. The defaults never seem to be nearly enough for Drupal. These go much larger, larger than you really need on production, but on local development it is easier to be safe than sorry.

```Dockerfile
# Increase resources for PHP
RUN sed -i "s/max_execution_time = 30/max_execution_time = 300/g" /usr/local/etc/php/php.ini \
    && sed -i "s/max_input_time = 60/max_input_time = 600/g" /usr/local/etc/php/php.ini \
    && sed -i "s/memory_limit = 128M/memory_limit = 2048M/g" /usr/local/etc/php/php.ini \
    && sed -i "s/upload_max_filesize = 2M/upload_max_filesize = 128M/g" /usr/local/etc/php/php.ini \
    && sed -i "s/post_max_size = 8M/post_max_size = 256M/g" /usr/local/etc/php/php.ini \
    && sed -i "s/;max_input_vars = 1000/max_input_vars = 10000/g" /usr/local/etc/php/php.ini
```

This is a minor thing, but I like when I grep through code to see results with the colour highlights, making it much easier to read. That can be done by setting an environment variable and putting an alias for grep into a bashrc file.

```Dockerfile
# Set up nicer grep results
ENV GREP_COLORS='mt=1;37;41'
COPY .bashrc /user/www-data/.bashrc
```

Line up the script that will be run after creating the images. That script will handle setting up the Drupal database, but I'll cover that more in the next post.

```Dockerfile
# Scripts for further actions to take on creation and attachment
COPY ./scripts/postCreateCommand.sh /postCreateCommand.sh
```

Copy the Drupal settings file and the local services file. The former is the essential settings for any Drupal site, including being able to connect to the database. The latter is not essential but enables some of the development features that are ideal in local development, like seeing comments in the generated HTML code that shows you what template is being used to generate it.

```Dockerfile
# Drupal configuration
COPY /drupal /web/sites
```

Finally, set the permissions on the main Drupal folder as well as on the script that we'll need to be able to run.

```Dockerfile
RUN chown -R www-data:www-data /opt/drupal \
    && chown www-data:www-data /postCreateCommand.sh \
    && chmod 777 /postCreateCommand.sh
```

Much of this is the same as the previous version: adding a self-signed SSL certificate, increasing PHP resources. But a few things changed:

- Many of the PHP extensions are already installed from the Drupal image so I didn't need to do them again in this Dockerfile.
- It needs a lot more overriding the permissions since that Drupal image already comes with a Drupal install in that /opt/drupal directory and with the www-data user, and with some as volumes which mean they immediately revert to root ownership when you attach to it.

## The Database Image

Here's the entire Dockerfile:

```Dockerfile
# Use the default MariaDB image
FROM mariadb:latest

ENV MARIADB_ROOT_PASSWORD=drupalroot
ENV MARIADB_DATABASE=drupal
ENV MARIADB_USER=drupal
ENV MARIADB_PASSWORD=drupal

# Expose the MySQL port to be accessible to the web container.
EXPOSE 3306
```

This is essentially no change from the previous one, just setting the environment variables to define the database and its access.

## Docker Compose

The docker compose file is not doing anything too unusual. The main thing of note is the volumes. I do not have the entire /opt/drupal set up as volumes. That's because when you attach to it, the entire area would get its permissions reassigned back to root, which causes issues. It's also not very efficient, since I really don't need things like the core files and contributed modules in the vendor folder to be persistent. So instead, it defines the pieces that are relevant to be able to build it, as well as the private and public folders where I might want to download a file for testing that the site can use.

I also have it set up to share my user profile's SSH keys and preferred settings like the my commit name, so that I can connect to the repository without having to configure it all again with every new build.

```yml
version: "3.9"
services:
  web:
    hostname: "web"
    container_name: "web"
    build:
      context: ".devcontainer"
      dockerfile: "web.Dockerfile"
    ports:
      - "443:443"
    volumes:
      - "./.devcontainer/:/opt/drupal/.devcontainer/"
      - "./.git/:/opt/drupal/.git/"
      - "./.gitignore:/opt/drupal/.gitignore"
      - "./.vscode/:/opt/drupal/.vscode/"
      - "./patches/:/opt/drupal/patches/"
      - "./private/:/opt/drupal/private/"
      - "./sync/:/opt/drupal/sync/"
      - "./web/sites/default/files/:/opt/drupal/web/sites/default/files/"
      - "./composer.json:/opt/drupal/composer.json"
      - "./composer.lock:/opt/drupal/composer.lock"
      - "./docker-compose.yml:/opt/drupal/docker-compose.yml"
      - "./README.md:/opt/drupal/README.md"
      - "${USERPROFILE}/.ssh/:/user/www-data/.ssh/"
      - "${USERPROFILE}/.gitconfig:/user/www-data/.gitconfig"
    user: "www-data:www-data"
    depends_on:
      - db
    networks:
      - "drupal"
  db:
    hostname: "db"
    container_name: "db"
    build:
      context: ".devcontainer"
      dockerfile: "db.Dockerfile"
    networks:
      - "drupal"

networks:
  drupal:
```

## Next Post

In the next post, I'll mention some of the changes to the devcontainer.json file and to the postCreateCommand script, although there's nothing too drastic there.