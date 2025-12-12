---
title: "Drupal GitPod Container 3: Settings and Config"
date: 2022-02-01T20:02:00.000Z
author: Ryan Robinson
description: "Settings and configuration for a Drupal developer environment in GitPod."
series: Drupal GitPod
tags:
  - Drupal
  - Git, GitLab, and GitHub
  - DevOps
  - Visual Studio Code
---

This post continues a [mini-series](/tags/gitpod-drupal/) in which I describe how I created a generic Drupal-friendly container working with GitPod. [The code is available in my GitHub](https://github.com/ryan-l-robinson/Drupal-GitPod).

The first two posts covered the two big files: GitPod.Dockerfile and .gitpod.yml. This final post will cover a few minor changes I had to make to other configuration files.

## Apache Conf

Only one thing needs to change from the default Apache configuration file: telling it to look at /web for the actual website content, instead of /var/www/html. That’s because a Drupal project typically has a top level that includes several necessary pieces of the site like the composer.json file, private files, and configuration sync files, outside the folder where the public site’s content is found.

That’s done by changing the document root from `"${GITPOD_REPO_ROOT}"` to `"${GITPOD_REPO_ROOT}/web"`, so the relevant section of the file looks something like this:

```apache
# Directory and files to be served
DirectoryIndex index.html index.htm index.php
DocumentRoot "${GITPOD_REPO_ROOT}/web"

RewriteCond  %{REQUEST_FILENAME} !^/$
RewriteCond  %{REQUEST_FILENAME} !^/(files|misc|uploads)(/.*)?
RewriteCond  %{REQUEST_FILENAME} !\.(php|ico|png|jpg|gif|css|js|html?)(\W.*)?
RewriteRule ^(.*)$ /index.php?q=$1 [L,QSA]

<Directory "${GITPOD_REPO_ROOT}/web">
    AllowOverride all
    Require all granted
</Directory>
```

To use this conf file, I saved my complete file in the .gitpod directory and then had the GitPod.Dockerfile copy that file into place:

```Dockerfile
COPY .gitpod/apache2.conf /etc/apache2/apache2.conf
```

## PHP.ini

The PHP.ini is similarly close to the default Ubuntu, but not quite. We’ll have to add support for a few extensions at minimum. There are a couple ways to do this. One approach is to have the script add the lines to enable the extensions, without touching anything else. The advantage of this is keeping everything else to the default, which may matter as the GitPod base image changes over time. That would look like:

```bash
echo "extension=uploadprogress" >> /etc/php/8.0/apache2/php.ini
```

The other alternative as I’ve done here is to completely overwrite the php.ini file with my own. The advantage of that approach is to also control all the other variables like the memory limit. The more you want to change from the default, the more this approach makes better sense than adding individual lines. So in that case, the file is copying using this line in the .gitpod.yml:

```bash
sudo cp /workspace/Drupal-GitPod/.gitpod/conf/php.ini /etc/php/8.0/apache2/php.ini
```

You could also accomplish this using sed to replace lines of the file in place, which is nice if you want to keep a perfectly-structured file replacing a default value. For example, to change the max\_execution\_time from 30s to 300s, I might do this:

```Dockerfile
RUN sed -i "s/max_execution_time = 30/max_execution_time = 300/g" /etc/php/8.0/apache2/php.ini
```

## Drupal Settings

Most of the Drupal settings are similar to the default. The largest change is familiar to any Drupal developer: set the database credentials. Since this is a GitPod container which gets created and destroyed on the fly regularly, I went simple using the root user and no password. A production server would have a different user and a good password.

```php
$databases['default']['default'] = array (
  'database' => 'drupal',
  'username' => 'root',
  'password' => '',
  'prefix' => '',
  'host' => '127.0.0.1',
  'port' => '3306',
  'namespace' => 'Drupal\\Core\\Database\\Driver\\mysql',
  'driver' => 'mysql',
);
```

Beyond that, there are a couple of tweaks both related to the fact that the GitPod domain is going to change.

First I set the trusted\_host\_patterns. The valid domains for browsing this site are either as some subdomain of gitpod.io, or as localhost.

```php
$settings['trusted_host_patterns'] = [
  '^.+\.gitpod\.io$',
  '^localhost$'
];
```

I also opened up the cookie\_domain to allow anything. This is so that it doesn’t object to you first logging into the site at one GitPod subdomain, then spinning up a new container and trying to login with that one.

```php
 /**
  * Used to track what domains you're logged into
  * Set to * to accept any GitPod domain
  */
$cookie_domain = '*';
```

That’s it! I’ve now covered having a functional Drupal-friendly GitPod container.
