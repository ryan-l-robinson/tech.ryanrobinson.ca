---
title: Pa11y and Pa11y-CI Accessibility
date: 2026-01-15T22:54:16.700Z
author: Ryan Robinson
description: "Pa11y and Pa11y-ci are a helpful accessibility testing tool, as part of your CI workflow or running locally."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
  - Accessibility
draft: true
---

This post is one of several in a series detailing my development environment and CI/CD deployment pipeline. [The code for the developer environment can be seen on my GitLab](https://gitlab.com/ryan-l-robinson/drupal-dev-environment). Other posts in the series can be seen by checking the Drupal Docker series links in the sidebar. I provided [an overview in the first post of the series](/2025/drupal-docker-deploys-overview/).

This series has now covered all the core functionality of having a local development environment and deploying updates. The majority of what's left has to do with automated testing, including this one using Playwright.

Pa11y-ci tests: building the image, storing it in the registry, then using it for CI/CD to run tests against the sitemap and report to HTML as an artifact.

## A Dedicated Testing Image

This project can create an image dedicated to running Playwright tests. That can be used for running tests like one against the sitemap, which will work externally as long as the site you're trying to test is publicly accessible.

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
