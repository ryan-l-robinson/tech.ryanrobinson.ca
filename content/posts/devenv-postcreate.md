---
title: "postCreateCommand for Dev Tools"
date: 2025-12-23T14:16:00.000Z
author: Ryan Robinson
description: "A script to run on the local dev environment including supporting Playwright, pa11y, and phpunit."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
  - Linux
  - Accessibility
draft: true
---
This post is one of several in a series detailing my development environment and CI/CD deployment pipeline. [The code for the developer environment can be seen on my GitLab](https://gitlab.com/ryan-l-robinson/drupal-dev-environment). Other posts in the series can be seen by checking the Drupal Docker series links in the sidebar. I provided [an overview in the first post of the series](/2025/drupal-docker-deploys-overview/).

In [the last post](/2025/devenv-devcontainer/), I detailed the devcontainer file. Among other things, it defines a script that runs the first time the environment is built. This post will look at that script.

## Playwright with Axe-Core

Playwright is a great tool for simulating browsers and running tests. Axe-Core can tie into that to check accessibility of the pages after other actions are taken. To be able to run Playwright in the dev environment, some packages are needed:

```bash
# On initial create, add the Playwright setup
npm install playwright @axe-core/playwright @playwright/test @types/node fsp-xml-parser
npm init playwright@latest --yes -- --no-overwrite --quiet --browser=chromium --browser=firefox --browser=webkit --lang=ts
npx playwright install --with-deps chromium firefox webkit
rm /opt/drupal/tests/example.spec.ts
rm -rf /opt/drupal/tests-examples
rm -rf /opt/drupal/e2e
```

## Pa11y and Pa11y-ci

Pa11y and Pa11y-ci are also accessibility testing tools. While they are based on the same testing criteria, pa11y and pa11y-ci do have some slightly different functions and there are some scenarios where I want each, so both are included.

```bash
# Also add pa11y and pa11y-ci
apt-get update
apt-get install -y --no-install-recommends \
    wget \
    xdg-utils \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

npm install pa11y pa11y-ci-reporter-html -g --unsafe-perm=true --allow-root
```

## PhpUnit

One minor thing is needed here to be able to run phpunit functional tests, which only work when running as www-data, not as root.

```bash
# phpunit functional tests need to run as www-data
echo -e '\nphpunit() {\n    su -s /bin/bash -c "vendor/bin/phpunit $@" www-data\n}' >> /root/.bashrc
source /root/.bashrc
```
