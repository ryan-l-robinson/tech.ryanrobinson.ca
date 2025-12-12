---
title: "Drupal Docker: The Dockerfiles"
date: 2022-06-24T01:46:00.000Z
author: Ryan Robinson
description: "The Dockerfile for a Drupal Docker setup."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
---

This continues a mini-series describing how I set up a Drupal development environment using [Docker Desktop and the VS Code devcontainer functionality](/tags/drupal-docker/). The full code is available in [my GitHub](https://github.com/ryan-l-robinson/Drupal-Devcontainer).

This post takes a look at the three Dockerfiles required:

## MariaDB

Let’s do the simple one first: the MariaDB container for the database. This is using a general official MariaDB image and I’m not even being picky about version. This is the entire file:

```Dockerfile
# Use the default MariaDB image, not a specific Oracle Linux one
FROM mariadb:latest

# Expose the MySQL port to be accessible to the web container.
EXPOSE 3306
```

## PHP

This image starts from Oracle Linux, a requirement of this particular hosting environment:

```Dockerfile
FROM oraclelinux:8
USER root
SHELL ["/bin/bash", "-c"]
```

It adds some other useful utilities, although they may not really be needed here since I won’t be accessing this container directly:

```Dockerfile
# Install other useful packages
RUN dnf install -y curl wget zip
```

It creates a directory that is necessary for running PHP-FPM:

```Dockerfile
# Create required directory
RUN mkdir -p /run/php-fpm
```

It installs PHP 8.0, which requires a different repository (at least so far):

```Dockerfile
# Install PHP 8.0, which needs a different repository
RUN dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
RUN dnf -y install https://rpms.remirepo.net/enterprise/remi-release-8.rpm
RUN dnf -y module reset php
RUN dnf -y module enable php:remi-8.0
RUN dnf -y install php
```

It adds several PHP extensions, including a few that are recommended by not required by Drupal (apcu and uploadprogress):

```Dockerfile
# Install PHP extensions, including PECL with APCU and UploadProgress extensions, recommended by Drupal
RUN dnf install -y php-gd php-pdo php-zip php-mysqlnd gcc make php-devel php-pear php-pecl-apcu php-pecl-uploadprogress
```

It installs XDebug, the PHP debugging tool:

```Dockerfile
# Install XDebug
RUN dnf install -y php-pecl-xdebug
RUN touch /var/log/xdebug.log
COPY /conf/xdebug.ini /etc/php.d/xdebug.ini
```

It makes several changes to php.ini to increase performance:

```Dockerfile
# Increase resources for PHP
RUN sed -i "s/max_execution_time = 30/max_execution_time = 300/g" /etc/php.ini
RUN sed -i "s/max_input_time = 60/max_input_time = 600/g" /etc/php.ini
RUN sed -i "s/memory_limit = 128M/memory_limit = 2048M/g" /etc/php.ini
RUN sed -i "s/upload_max_filesize = 2M/upload_max_filesize = 128M/g" /etc/php.ini
RUN sed -i "s/post_max_size = 8M/post_max_size = 0/g" /etc/php.ini
RUN sed -i "s/display_errors = Off/display_errors = On/g" /etc/php.ini
RUN echo "# Increase timeout" >> /etc/httpd/conf.d/php.conf
RUN echo "Timeout 1200" >> /etc/httpd/conf.d/php.conf
RUN echo "ProxyTimeout 1200" >> /etc/httpd/conf.d/php.conf
```

It updates the php-fpm configuration to allow listeners from other clients (the web container):

```Dockerfile
# Update PHP-FPM configuration including allowing listeners from other clients, defining the OPCache, and listening on port 9000
RUN sed -i "s/listen.allowed_clients = 127.0.0.1/;listen.allowed_clients = 127.0.0.1/g" /etc/php-fpm.d/www.conf
RUN sed -i "s/;php_value[opcache.file_cache]  = \/var\/lib\/php\/opcache/php_value[opcache.file_cache]  = \/var\/lib\/php\/opcache/g" /etc/php-fpm.d/www.conf
RUN sed -i "s/listen = \/run\/php-fpm\/www.sock/listen = 9000/g" /etc/php-fpm.d/www.conf
```

And finally, it starts php-fpm as the service running in the foreground for this container:

```Dockerfile
# Start PHP-FPM
ENTRYPOINT ["php-fpm"]
CMD ["-F", "-R"]
```

## Apache

The final container, web, is the main one that the devcontainer will connect to, running Apache as its main service. Much of this repeats the PHP container, so I won’t go through all that again. Here’s what’s different.

It includes a few more useful packages:

```Dockerfile
# Install other needed packages
RUN dnf install -y curl wget git zip mod_ssl httpd php-gd openssl which mariadb sudo patch vim
```

It configures Apache, including creating a self-signed SSL certificate for https browsing:

```Dockerfile
# Apache configuration, including SSL certificates and logs
RUN mkdir -p /etc/httpd/certs
COPY /conf/openssl-config.txt /etc/httpd/certs/openssl-config.txt
RUN openssl req -batch -newkey rsa:4096 -nodes -sha256 -keyout /etc/httpd/certs/local.drupal.com.key -x509 -days 3650 -out /etc/httpd/certs/local.drupal.com.crt -config /etc/httpd/certs/openssl-config.txt
RUN openssl req -batch -newkey rsa:4096 -nodes -sha256 -keyout /etc/pki/tls/private/localhost.key -x509 -days 3650 -out /etc/pki/tls/certs/localhost.crt -config /etc/httpd/certs/openssl-config.txt
RUN mkdir -p /var/log/local.drupal.com
COPY /conf/local.drupal.com.conf /etc/httpd/conf.d/local.drupal.com.conf
```

It adds the drupal user and alters some other permissions so it can edit the web folder directly without sudo, and can sudo when necessary:

```Dockerfile
# Add drupal user within the apache group
RUN useradd drupal
RUN usermod -aG apache drupal
# Change default permissions to 775 instead of 755, so that the drupal user can write to the web root
RUN umask 0002
# Create sudo group, add drupal user to it, and allow those users to sudo without password
RUN groupadd sudo
RUN usermod -aG sudo drupal
RUN sed -i "s/# %wheel ALL=(ALL) NOPASSWD: ALL/%sudo ALL=(ALL) NOPASSWD: ALL/g" /etc/sudoers
```

It installs composer, an essential for Drupal management:

```Dockerfile
# Install latest composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php --install-dir /usr/bin --filename composer
RUN php -r "unlink('composer-setup.php');"
ENV COMPOSER_PROCESS_TIMEOUT=9999
```

It installs pa11y for accessibility testing:

```Dockerfile
# Install pa11y accessibility testing tool, including NodeJS
RUN dnf install -y nodejs pango.x86_64 libXcomposite.x86_64 libXdamage.x86_64 libXext.x86_64 libXi.x86_64 libXtst.x86_64 cups-libs.x86_64 libXScrnSaver.x86_64 libXrandr.x86_64 GConf2.x86_64 alsa-lib.x86_64 atk.x86_64 gtk3.x86_64 nss libdrm libgbm xorg-x11-fonts-100dpi xorg-x11-fonts-75dpi xorg-x11-utils xorg-x11-fonts-cyrillic xorg-x11-fonts-Type1 xorg-x11-fonts-misc libxshmfence
RUN npm install pa11y -g --unsafe-perm=true --allow-root
```

It adds executable permissions to the postCreateCommand script which I will detail in a later post:

```Dockerfile
# Scripts for further actions to take on creation and attachment
COPY ./scripts/postCreateCommand.sh /postCreateCommand.sh
RUN ["chmod", "+x", "/postCreateCommand.sh"]
```

And finally, it runs Apache as its primary foreground process:

```Dockerfile
# Start Apache
ENTRYPOINT ["/usr/sbin/httpd"]
CMD ["-D", "FOREGROUND"]
```
