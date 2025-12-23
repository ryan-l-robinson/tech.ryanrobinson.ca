---
title: Playwright Testing
date: 2026-01-02T22:54:16.700Z
author: Ryan Robinson
description: Playwright is a helpful testing tool, both locally and in your CI processes.
series: Drupal Docker
tags:
  - Drupal
  - DevOps
  - Accessibility
  - JavaScript
draft: true
---

This post is one of several in a series detailing my development environment and CI/CD deployment pipeline. [The code for the developer environment can be seen on my GitLab](https://gitlab.com/ryan-l-robinson/drupal-dev-environment). Other posts in the series can be seen by checking the Drupal Docker series links in the sidebar. I provided [an overview in the first post of the series](/2025/drupal-docker-deploys-overview/).

This series has now covered all the core functionality of having a local development environment and deploying updates. The majority of what's left has to do with automated testing, including this one using Playwright.

[Playwright](https://playwright.dev/) is a tool that can simulate browser activity and then assert that things are how you think they should be. That includes interactions, like expanding a menu or clicking on a button. It also includes the ability to emulate multiple browsers and operating systems and screen sizes and dark or light mode.

## A Dedicated Testing Image

This project can create an image dedicated to running Playwright tests. That can be used for running tests like one against the sitemap, which will work externally as long as the site you're trying to test is publicly accessible.

## Running Tests Locally

### Installation Requirements

These were already covered in previous posts, specifically the postCreateCommand, combined with the Playwright extension for VS Code being enabled.

### Using Tests Explorer in VS Code

These tests, like many others, are integrated within the Tests Explorer functionality of VS Code. You can use that to see all tests and their status at a glance, and run more.

### Writing Tests

Tests can be written in TypeScript or plain JavaScript, neither of which are my specialty.
