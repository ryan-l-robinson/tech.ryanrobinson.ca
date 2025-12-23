---
title: PHP Lint and CS Testing
date: 2026-01-01T13:16:00.000Z
author: Ryan Robinson
description: "PHP Linting and CS Testing as CI/CD jobs to confirm you did not introduce a major error."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
  - PHP
draft: true
---
This post is one of several in a series detailing my development environment and CI/CD deployment pipeline. [The code for the developer environment can be seen on my GitLab](https://gitlab.com/ryan-l-robinson/drupal-dev-environment). Other posts in the series can be seen by checking the Drupal Docker series links in the sidebar. I provided [an overview in the first post of the series](/2025/drupal-docker-deploys-overview/).

This series has now covered all the core functionality of having a local development environment and deploying updates. This leaves a few posts to go related to automated testing, including this one on PHP linting and CodeSniffer, as well as a post on the database lifecycle system to copy production content back to other environments occasionally so they are more representative of tests.

Let's start diving in to PHP linting and PHP CodeSniffer, a couple of tests to avoid errors in PHP code.

## PHP Lint

PHP linting checks for any fatal errors. These kinds of errors are a lot harder to keep in the code as far as committing it into GitLab in full dedicated developer environments with things like syntax and error highlighting, because you're probably going to notice it first. With that said, it is still a good failsafe to have, and it doesn't take that long to run.

Here's a GitLab CI/CD approach to scan any PHP changes in case of any fatal error.

## PHP CodeSniffer

CodeSniffer helps enforce more strict coding standards.
