---
title: "GitLab DevOps: PHP Lint"
date: 2022-02-05T13:51:00.000Z
author: Ryan Robinson
description: "Running a PHP lint test as part of a GitLab DevOps workflow."
tags:
  - Git, GitLab, and GitHub
  - DevOps
  - PHP
---

Here’s another piece in a [GitLab DevOps](/tags/gitlab-devops/) setting: when code is committed to GitLab, I want to run a PHP linter on the custom code folders of Drupal (modules and themes) to make sure there aren’t any glaring syntax bugs that snuck through. My personal favourite error is when a “:wq” gets inserted into a file trying to exit vim, after doing all the testing.

I split this into two pieces, taking advantage of [GitLab’s ability to extend other functions, even functions in other projects](https://docs.gitlab.com/ee/ci/yaml/index.html#extends). My real work scenario is using GitLab, hence using GitLab CI. For the purposes of sharing here, I’ve put them in [my personal GitHub](https://github.com/ryan-l-robinson/GitLab-CI-CD). I know that’s an unintuitive combination.

## The Generic CI/CD Functions

To be able to test, I first need a general Oracle Linux 8 image that can run PHP 8.0 and composer for a Drupal site.

```yml
## Build an Oracle Linux 8 container, used by other tests below ##
.ol8_lamp_build:
  stage: test
  image: oraclelinux:8
  before_script:
    - dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
    - dnf -y install https://rpms.remirepo.net/enterprise/remi-release-8.rpm
    - dnf -y module enable php:remi-8.0
    - dnf install -y php php-gd php-pdo zip unzip git php-curl php-mbstring php-zip php-json php-xml php-simplexml php-mysqlnd php-pecl-apcu wget curl
    - php -v
    - php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
    - php composer-setup.php
    - php -r "unlink('composer-setup.php');"
```

Part one is a project containing a few generic CI job templates. One of those, in the tests.yml file, is a PHP lint test. Saying that I want to test every php file in a certain folder is not as easy as you’d think – there is no flag for applying it recursively – so it requires a loop:

```yml
### PHP lint test ###
.php_lint:
  stage: test
  extends: .ol8_lamp_build
  variables:
    DIRECTORIES: "./"
    EXTENSIONS: "php"
  script:
    ## Recursively checks for files of specified extensions in specified directories and completes php lint on them
    - cwd="$(pwd)"
    - |
      for DIRECTORY in $DIRECTORIES 
        do
          cd $DIRECTORY
          for EXT in $EXTENSIONS
            do
              files="$(find -name *.${EXT} -type f)"

              for file in ${files}
                do php -l ${file};
              done;
            done;
          cd $cwd;
        done;
```

This has variables for the directory to start testing from, as well as what extensions to test. The extension variable defaults to only php files, but it can be overridden to include other extensions, which is valuable with Drupal where other extensions get used for php code like .module and .theme.

## The Project’s CI File

Part two is the specific project that then references those generic templates. Using the full project reference, under the assumption that in a real situation this would be in a differernt GitLab project, that looks something like this in the project’s .gitlab-ci.yml file:

```yml
# Includes general CI jobs to extend from
include:
 
    - project: '[path to the GitLab project with CI]'
      ref: main
      file: test.yml

## Stages ##
stages:
  - test

## Test Jobs ##

### Tests for PHP errors in custom code ###
php:
  extends: .php_lint
  variables:
    DIRECTORIES: ./web/themes/custom ./web/modules/custom
    EXTENSIONS: php module theme inc
```

This example will run the PHP syntax test against all .php, .module, .theme, and .inc files located with the Drupal 8+ custom themes and custom modules directories.
