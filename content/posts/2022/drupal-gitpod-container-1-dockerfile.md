---
title: "Drupal GitPod Container 1: .Dockerfile"
date: 2022-01-20T16:00:48.000Z
author: Ryan Robinson
tags:
    - Drupal
    - Git, GitLab, and GitHub
    - DevOps
    - GitPod Drupal
    - PHP
    - Visual Studio Code
    - Linux
series: GitPod Drupal
---

[GitPod](https://gitpod.io) is a great tool for cloud-based containers when developing. If you’re developing and want a safe and efficient cloud container to try things out, it’s a pretty good way to go. You even get 50 hours per month for free, which is pretty great if you only need occasional side project and not full-time work. It also works with [Visual Studio Code](/tags/visual-studio-code/) – although that has not been working for me lately – so you can use it in the browser or in your desktop editor. When you browse to a GitHub or GitLab repository with the extension installed, there’s a simple button that will launch the container with that repository’s code, making it quick and easy to see how it works as well as make changes.

In this [mini-series](/tags/gitpod-drupal/) I describe how I created a generic Drupal-friendly container working with GitPod. [It is available in my GitHub](https://github.com/ryan-l-robinson/Drupal-GitPod). Note that since this is some code I may continue using over time, the code there may change beyond what is covered in this article.

## The Dockerfile

This solution starts from [the GitPod-provided MySQL image](https://github.com/gitpod-io/workspace-images/blob/master/mysql/Dockerfile). That does not give everything you need for a functioning Drupal site, but it does meet the basics of a LAMP stack.

```Dockerfile
FROM gitpod/workspace-mysql
```

The biggest catch with using this image is that you might end up with the latest PHP version, and there’s a good chance that your Drupal site doesn’t support that yet. So you probably want to add a section to change the PHP to one that you know currently works:

```Dockerfile
RUN update-alternatives --set php /usr/bin/php8.0
```

That doesn’t cover everything, though. There are a few more things I want for every Drupal 9+ site.

### PHP packages

Here’s how to add PHP packages needed within an Ubuntu Dockerfile, as well as APCU and uploadprogress which are also recommended by Drupal. I wrote recently about [installing PECL UploadProgress for Oracle Linux](/posts/2021/drupal-pecl-uploadprogress/), which is similar but a bit different than these Ubuntu commands.

```Dockerfile
USER root
# Install other needed packages
RUN apt update
RUN apt install -y php-pear php-apcu php-json php-xdebug build-essential mysql-client sendmail
RUN pecl install apcu
RUN pecl install uploadprogress
```

### Composer

Drupal can work without composer, [but your life is going to be a lot easier with it](https://www.drupal.org/docs/develop/using-composer/using-composer-with-drupal). Here’s how I added that to my Dockerfile:

```Dockerfile
# Install latest composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === '906a84df04cea2aa72f40b5f787e49f22d4c2f19492ac310e8cba5b96ac8b64115ac402c8cd292b8a03482574915d1a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php --install-dir /usr/bin --filename composer
RUN php -r "unlink('composer-setup.php');"
```

At this point, the server is ready to provide the core packages that run Drupal well.

## Pa11y accessibility testing

[I’ve also mentioned pa11y recently](/posts/2021/pa11y-ci-oracle-linux-8-installation/), the accessibility testing tool. I always want that available. Here’s how to do that in this Ubuntu-based Dockerfile:

```Dockerfile
# Install pa11y accessibility testing tool, including NodeJS
RUN curl -sL https://deb.nodesource.com/setup_16.x -o # Install pa11y accessibility testing tool, including NodeJS and Chromium
RUN curl -sL https://deb.nodesource.com/setup_16.x -o nodesource_setup.sh
RUN bash nodesource_setup.sh
RUN apt-get install -y nodejs libnss3 libatk1.0-0 libatk-bridge2.0-0 libcups2 libdrm-amdgpu1 libxkbcommon-x11-0 libxcomposite-dev libxdamage-dev libxrandr-dev libgbm-dev libgtk-3-common libxshmfence-dev software-properties-common
RUN apt-get install -y apparmor snapd apparmor-profiles-extra apparmor-utils kdialog chromium-browser libappindicator1 fonts-liberation
RUN npm install pa11y -g --unsafe-perm=true --allow-root
```

### Ports

Finally, I’m going to open the standard ports: 80 (HTTP), 443 (HTTPS), and 3306 (MySQL). This doesn’t seem to be actually necessary but it can help to remind yourself what ports you’re using.

```Dockerfile
EXPOSE 80
EXPOSE 443
EXPOSE 3306
```

## What’s Next

In an upcoming post, I’ll continue with the other large file to make this work: the .gitpod.yml. A third post will cover a few smaller changes I had to make to other configuration.
