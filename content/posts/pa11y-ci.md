---
title: Pa11y and Pa11y-CI Accessibility
date: 2026-01-14T22:54:16.700Z
author: Ryan Robinson
description: "Pa11y and Pa11y-ci are a helpful accessibility testing tool, as part of your CI workflow or running locally."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
  - Accessibility
draft: true
---

This series has now covered all the core functionality of having a local development environment and deploying updates. The majority of what's left has to do with automated testing, including this one using Pa11y and Pa11y-CI.

Pa11y and Pa11y-CI, as the names suggest, are very similar with the same kinds of tests it can run, but with a few differences in exactly which features are available. The biggest one of interest to me is that Pa11y-CI includes an extremely simple command out of the box to check the accessibility of all pages in a sitemap.

One notable difference compared to the Playwright tests from the last post is that there are no other interactions or mock devices involved here. Where Playwright I could say things like "using an iPhone X in dark mode, click on this button and then check the accessibility of the page afterward" here it is much more simply scanning every page. That gives it an advantage in ease of use, but doesn't quite cover every scenario you may want to test.

## CI/CD Tests

### Building A Dedicated Testing Image

[A dedicated pa11y testing image is available in my GitLab](https://gitlab.com/ryan-l-robinson/pa11y-Docker-GitLab). This project can create an image dedicated to running Pa11y-CI tests. That can be used for running tests like one against the sitemap, which will work externally as long as the site you're trying to test is publicly accessible.

The Dockerfile installs some needed packages, sets up the necessary file structure, and installs pa11y-ci including with the HTML reporter that is the most friendly to read for a large site.

```Dockerfile
FROM debian:12-slim

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    nodejs \
    npm \
    ca-certificates \
    fonts-liberation \
    libasound2 \
    libatk-bridge2.0-0 \
    libatk1.0-0 \
    libatspi2.0-0 \
    libc6 \
    libcairo2 \
    libcups2 \
    libdbus-1-3 \
    libdrm2 \
    libexpat1 \
    libgbm1 \
    libglib2.0-0 \
    libgtk-3-0 \
    libnspr4 \
    libnss3 \
    libpango-1.0-0 \
    libpangocairo-1.0-0 \
    libstdc++6 \
    libudev1 \
    libuuid1 \
    libx11-6 \
    libx11-xcb1 \
    libxcb-dri3-0 \
    libxcomposite1 \
    libxcursor1 \
    libxdamage1 \
    libxext6 \
    libxfixes3 \
    libxi6 \
    libxkbcommon0 \
    libxrandr2 \
    libxrender1 \
    libxshmfence1 \
    libxss1 \
    libxtst6 \
    wget \
    xdg-utils \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# mkdir for the application tests
RUN mkdir -p /opt/pa11y
WORKDIR /opt/pa11y

# Install pa11y-ci and the HTML reporter
RUN npm install -g pa11y-ci pa11y-ci-reporter-html

# Copy config file
COPY / /opt/pa11y/
```

As always in this Drupal Docker series, the gitlab-ci job then extends [my general purpose Docker build job that I've already gotten into](https://tech.ryanrobinson.ca/2025/devenv-build-image-ci/).

It also has a configuration file that defines how pa11y-ci should run, including that it will report to both the command line and to the HTML reporter. The most important here is the --no-sandbox option. None of the tests will run without that.

```json
{
    "defaults": {
        "timeout": 120000,
        "reporters": [
            "cli",
            "pa11y-ci-reporter-html"
        ],
        "chromeLaunchConfig": {
            "args": [
                "--no-sandbox"
            ]
        },
        "runners": [
            "axe",
            "htmlcs"
        ],
        "ignore": [
            "frame-tested",
            "frame-title"
        ],
        "level": "warning"
    }
}
```

### Running the Tests

When I want a specific project to be able to run tests, the CI job in that project would look like:

```yml
# Includes general CI jobs #
include:
  - project: "ryan-l-robinson/gitlab-ci"
    ref: main
    file: test.yml

### Checks a sitemap using Pa11y. ###
pa11y_test:
  extends: .pa11y_test
  stage: content_test
  environment: Production
  variables:
    SITEMAP_URL: https://tech.ryanrobinson.ca/sitemap.xml
  only:
    refs:
      - main
```

That is extending my general purpose job which uses the image I've previously built, checking all the pages of a sitemap, and keeping the report for 5 months:

```yml
## Runs accessibility sitemap tests using pa11y ##
.pa11y_test:
  stage: test
  image: registry.gitlab.com/ryan-l-robinson/pa11y-docker-gitlab/pa11y
  variables:
    SITEMAP_URL: ""
  script:
    # Use pa11y-ci's built-in function for scanning from a sitemap. #
    - pa11y-ci --sitemap $SITEMAP_URL --config /opt/pa11y/config.json
  # Show as warning but don't stop execution of later jobs if failure. #
  allow_failure: true
  # Save the reports for 5 months, past our next release. #
  artifacts:
    when: always
    paths:
      - pa11y-ci-report
    expire_in: 5 months
```

## Running Tests Locally

### Installation Requirements

These were already covered in previous posts, specifically the postCreateCommand.

### Running Tests

These tests can only be run from the command line, like:

```bash
pa11y
```

Or using pa11y-ci to check an entire sitemap:

```bash
pa11y-ci --sitemap https://tech.ryanrobinson.ca
```

### HTML Reporter

Pa11y can dump the results into reports in a few different formats. The easiest to read, especially if it is a large site, is to dump it into HTML files. This then allows browsing the results one at a time in a very easy to read format. Some of the other formats like a JSON file aren't bad if there are few results, but get hard to work with on larger sites.

### Configuration

Unlike Playwright, there are no tests to write yourself here, but you may need to adjust some of the configuration files. Pa11y and Pa11y-ci have different files.
