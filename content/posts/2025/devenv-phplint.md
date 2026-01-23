---
title: PHP Lint and CS Testing
date: 2025-12-23T16:05:00.000Z
author: Ryan Robinson
description: "PHP Linting and CS Testing as CI/CD jobs to confirm you did not introduce a major error."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
  - PHP
---
This post is one of several in a series detailing my development environment and CI/CD deployment pipeline. [The code for the developer environment can be seen on my GitLab](https://gitlab.com/ryan-l-robinson/drupal-dev-environment). Other posts in the series can be seen by checking the Drupal Docker series links in the sidebar. I provided [an overview in the first post of the series](/2025/drupal-docker-deploys-overview/).

This series has now covered all the core functionality of having a local development environment and deploying updates. The majority of what's left has to do with automated testing, including this one where I will start to look at PHP linting and PHP CodeSniffer, a couple of tools to help detect errors in PHP code.

## PHP Lint

PHP linting checks for any fatal errors. These kinds of errors are a lot harder to keep in the code long enough to commit it into GitLab if you were using a full dedicated developer environments with things like syntax and error highlighting, because you're probably going to notice it first. With that said, it is still a good failsafe to have, and it doesn't take that long to run.

Here's a GitLab CI/CD approach to scan any PHP changes in case of any fatal error, starting with a general extendable job which will scan all files recursively in any specified directories.

```yml
.php_lint:
  stage: test
  image: drupal:php8.3-apache
  variables:
    DIRECTORIES: "./"
    EXTENSIONS: "php"
  script:
    # Recursively checks for files of specified extensions in specified directories and completes php lint on them. #
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

Each project can then set up up a CI/CD job to implement it, like this one that runs on any custom code change as well as on dev and staging, checking each of those custom code directories:

```yml
include:
  - project: "ryan-l-robinson/gitlab-ci"
    ref: main
    file: test.yml

## Stages ##
stages:
  - test

php_lint:
  extends: .php_lint
  variables:
    DIRECTORIES: ./web/themes/custom ./web/modules/custom
    EXTENSIONS: php module theme inc
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
      when: never
    - if: '$CI_COMMIT_BRANCH == "dev" || $CI_COMMIT_BRANCH == "staging"'
    - changes:
        - web/modules/custom/**/*
        - web/themes/custom/**/*
```

## PHP CodeSniffer

CodeSniffer helps enforce more strict coding standards. There are standards defined for both Drupal and DrupalPractice. Drupal covers a lot of the basic bare minimum compliance with Drupal standards: indentation, style, and file structure. DrupalPractice goes further to offer deeper best practices.

This one is a one-line script, so I haven't bothered to split it into a dedicated separate template. This is just one job, in the project's .gitlab-ci.yml:

```yml
### CodeSniffer for coding style ###
php_cs:
  stage: php_built_test
  image: $CI_REGISTRY_IMAGE/web:$CI_COMMIT_REF_SLUG
  script:
    - /opt/drupal/vendor/bin/phpcs --standard="Drupal,DrupalPractice" -n --extensions="php,module,inc,install,test,theme" web/themes/custom web/modules/custom --ignore=*/vendor/*
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
      when: never
    - if: '$CI_COMMIT_BRANCH == "dev"'
    - if: '$CI_COMMIT_BRANCH == "staging" || $CI_COMMIT_BRANCH == "main"'
      when: never
    - changes:
        - web/modules/custom/**/*
        - web/themes/custom/**/*
```

The key thing to note here, unlike with PHP linting, is that this needs to run after an updated web image is built. PHP linting is looking for the kind of errors that are visible simply by reading the code, so it can run before the build and if something fails, you don't even need to waste the time on the build. PHP CodeSniffer, though, needs some vendor files which only happen when the composer packages are installed, which is normally part of the build process.
