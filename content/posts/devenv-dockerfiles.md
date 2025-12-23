---
title: "The Primary Web Dockerfile"
date: 2025-12-23T13:16:00.000Z
author: Ryan Robinson
description: "A Dockerfile for a Drupal dev environment including Apache, PHP, and XDebug, with some variation by environment."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
  - Linux
draft: true
---

This post is one of several in a series detailing my development environment and CI/CD deployment pipeline. [The code for the developer environment can be seen on my GitLab](https://gitlab.com/ryan-l-robinson/drupal-dev-environment). Other posts in the series can be seen by checking the Drupal Docker series links in the sidebar. I provided [an overview in the first post of the series](/2025/drupal-docker-deploys-overview/).

## The Web Image

The image is based on [the official Drupal image](https://hub.docker.com/_/drupal/), because that will provide a good amount of what is necessary without needing to repeat it.

```Dockerfile
FROM drupal:php8.3-apache
```

### Variables

Next are some arguments which are going to be passed through from the GitLab CI/CD job. BASE_URL is needed for some accessibility tests. BRANCH is needed to distinguish what to build since that isn't all the same in each environment. TIMESTAMP is used as a deployment identifier, which I then have in use for the environment indicator in Drupal as a way to display when it was last rebuilt. All three will get passed through again as environment variables.

```Dockerfile
ARG BASE_URL
ARG BRANCH
ARG TIMESTAMP

# Needed as an ENV for the start.sh script
ENV DEPLOY_ENV=$BRANCH
# Variables used for accessibility testing
ENV BASE_URL=$BASE_URL
# Variable used to differentiate deployments
ENV DEPLOYMENT_ID=$TIMESTAMP
```

A few more environment variables will help squash some warnings:

```Dockerfile
ENV DEBIAN_FRONTEND=noninteractive
ENV LC_ALL=en_CA.UTF-8
ENV LANG=en_CA.UTF-8
ENV LANGUAGE=en_CA.UTF-8
```

### General Packages

Install the extra packages - some of the most essential ones are already included in the Drupal image - and PHP development settings. In this first block, the only variance by environment is that main does not get nodejs or npm, which is needed for some things like playwright accessibility testing but that is not going to be running from production.

```Dockerfile
# Install needed repositories and general packages
RUN apt-get -y update && \
    apt-get -y --no-install-recommends install \
    default-mysql-client \
    locales \
    locales-all \
    gettext-base \
    git \
    openssh-client \
    unzip \
    zip
RUN if [ "$BRANCH" != "main" ]; then \
      apt-get -y --no-install-recommends install \
      nodejs \
      npm \
    ; fi
RUN apt-get clean && \
  rm -rf /var/lib/apt/lists/*
```

### PHP

Next up is PHP. There is a version of PHP already installed - that's the 8.3 specified in the image name at the start - but there are a few more things we need to improve.

```Dockerfile
# PHP.ini file varies by environment
RUN if [ "$BRANCH" != "main" ] && [ "$BRANCH" != "staging" ]; then \
      ln /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini \
    ; \
    else \
      ln /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini \
    ; fi

# Install PHP extensions, using PECL
RUN pecl channel-update pecl.php.net \
    && pecl install apcu uploadprogress \
    && docker-php-ext-enable apcu \
    && docker-php-ext-enable uploadprogress

COPY php/docker-php-ext-apcu.ini /usr/local/etc/php/conf.d/docker-php-ext-apcu.ini

# PHP.ini file varies by environment
RUN if [ "$BRANCH" != "main" ] && [ "$BRANCH" != "staging" ]; then \
      ln /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini \
    ; \
    else \
      ln /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini \
    ; fi

# Increase resources for PHP
RUN sed -i "s/max_execution_time = 30/max_execution_time = 300/g" /usr/local/etc/php/php.ini \
    && sed -i "s/max_input_time = 60/max_input_time = 600/g" /usr/local/etc/php/php.ini \
    && sed -i "s/memory_limit = 128M/memory_limit = 2048M/g" /usr/local/etc/php/php.ini \
    && sed -i "s/upload_max_filesize = 2M/upload_max_filesize = 128M/g" /usr/local/etc/php/php.ini \
    && sed -i "s/post_max_size = 8M/post_max_size = 256M/g" /usr/local/etc/php/php.ini \
    && sed -i "s/;max_input_vars = 1000/max_input_vars = 10000/g" /usr/local/etc/php/php.ini

# XDebug should only be on local/dev branches
RUN if [ "$BRANCH" != "main" ] && [ "$BRANCH" != "staging" ]; then \
    pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && touch /var/log/xdebug.log \
    && chown www-data:www-data /var/log/xdebug.log \
    && cp /opt/drupal/php/docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d \
    ; fi
```

Note the COPY of a file for the APCU configuration. That's a simple file, not worth its own post, and is really just necessary to get a higher memory limit than the default.

```ini
extension=apcu
apc.enable_cli=1
apc.shm_size=64M
```

There's also a similar xdebug configuration file being copied, which is similarly relatively straightforward. XDebug is a great tool for debugging with PHP.

```ini
zend_extension=xdebug.so
xdebug.mode=debug,coverage,profile
xdebug.start_with_request = trigger
xdebug.client_host = 127.0.0.1
xdebug.client_port = 9003
xdebug.log = /var/log/xdebug.log
```

### Apache

The other major component is Apache. As with PHP, the basics are already installed from the inherited base image, but it doesn't have any SSL certificate so you can't browse it as https. This section, then, will add a self-signed certificate for the Apache configuration, so that we'll be able to browse the site locally with HTTPS.

```Dockerfile
# Apache configuration, including SSL certificates and logs
COPY apache /etc/apache2

RUN a2enmod ssl

RUN openssl req \
    -batch \
    -newkey rsa:4096 \
    -nodes \
    -sha256 \
    -keyout /etc/apache2/certs/drupal.devenv.key \
    -x509 \
    -days 3650 \
    -out /etc/apache2/certs/drupal.devenv.crt \
    -config /etc/apache2/certs/openssl-config.txt && \
  chown -R root:www-data /etc/apache2 && \
  chmod 660 /etc/apache2/certs/*

# Set www-data file permissions
COPY --chown=www-data:www-data . /opt/drupal
```

Note that a self-signed certificate may not be sufficient, depending on what you are using for the database image. There are ways to give it a trusted certificate instead, shared with the database image. I am not getting into that here.

### Grep

This is a minor thing, but I like when I grep through code to see results with the colour highlights, making it much easier to read. That can be done by setting an environment variable and putting an alias for grep into a bashrc file.

```Dockerfile
# Set up nicer grep results
ENV GREP_COLORS='mt=1;37;41'
COPY .bashrc /user/www-data/.bashrc
```

### Permissions and Scripts

Give the permissions on the main folder to the www-data user that Apache runs as, and allow www-data to run the scripts:

```Dockerfile
RUN chown -R www-data:www-data /var/www /opt
RUN chmod -R 770 /opt/drupal/scripts
```

Those scripts will get more attention in a future post.

### Build the Composer Packages

Build the composer packages as the www-data user. There is some distinction for dev packages to not be included on main and staging.

```Dockerfile
# Get and build the Drupal code, with proper permissions
USER www-data
RUN if [ "$BRANCH" != "main" ] && [ "$BRANCH" != "staging" ]; then \
      composer install \
    ; \
    else \
      composer install --no-dev \
    ; fi
```

### The Start Script

Finally, designate the script that runs when container starts up. Again, I'll cover the details of that script in a later post:

```Dockerfile
CMD ["/opt/drupal/scripts/start.sh"]
```

## More to Come

There are some more pieces that I will need to get to in unpacking how this developer environment is set up, including:

- The docker-compose, which is actually a two-file setup with one for all environments plus one that is only for local.
- Devcontainer for VS Code configuration.
- The start script and some other related scripts for how it gets started up.
- The postCreateCommand script only for local to add Playwright and other tweaks.
- CI/CD build job: to build the image.
- CI/CD prune job: to clean up the build runner.
- CI/CD test jobs: phplint and phpcs.
- phpunit tests, local and in CI/CD.
- Playwright tests, local and in CI/CD.
- Pa11y tests, local and in CI/CD.
- Database lifecycle to occasionally bring production database content back to other environments.
