---
title: "Drupal: Install PECL UploadProgress"
date: 2021-12-10T12:55:00.000Z
author: Ryan Robinson
description: "Drupal needs PECL UploadProgress. This post will help you install it."
tags:
    - Drupal
    - Linux
    - PHP
---

Drupal often recommends that you install and enable PECL UploadProgress to show better feedback when uploading files, but it doesn’t make it obvious how to do that. This quick post will run through what it took for me to get PECL UploadProgress on an Oracle Linux 8 server.

## Add Required Packages

There are a few preliminary packages that you’ll need: gcc, make, php-devel, and php-pear. You can install in Oracle Linux 8 using this command:

```bash
sudo yum -y install gcc make php-devel php-pear
```

You don’t have to provide the -y. That will pre-approve any other prompts that come up, including if it requires installing other dependent packages. If you want to be more careful, don’t use that, but it can come in handy in contexts like installing this through a script.

## Install PECL

Now that you’ve got the dependencies, you can install uploadprogress:

```bash
sudo pecl -y install uploadprogress
```

## Add to php.ini

The packages are now in place, but your PHP installation will not make use of it unless it is recognized as an extension in your php.ini file. In the default Oracle Linux 8 setup, this file is located at /etc/php.ini. You’ll need to add this line to your file:

```text
extension=uploadprogress
```

You can edit that file directly using your preferred editor to add this line to the file, or if you are using this within a script, you can have the script write it to the end of the file like this:

```bash
sudo echo "extension=uploadprogress" >> /etc/php.ini
```

If you first need to check whether that line is already in the file – so that you don’t end up writing it multiple times in a context where a script might add another line every time – you can use this in a bash script:

```bash
if [ ! -z $(grep "extension=uploadprogress" /etc/php.ini) ]; then
    sudo echo "extension=uploadprogress" >> /etc/php.ini
fi
```

## Restart

Finally, you’ll need to restart Apache and PHP for this to go into effect:

```bash
sudo systemctl restart php-fpm
sudo systemctl restart httpd
```

[This script is available in my GitHub](https://github.com/ryan-l-robinson/Drupal-Oracle-Linux-scripts/tree/main).
