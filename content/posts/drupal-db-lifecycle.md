---
title: Database Lifecycle
date: 2026-01-09T22:54:16.700Z
author: Ryan Robinson
description: "I developed a semi-automated method to keep databases roughly in sync across environments."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
draft: true
---

This post is one of several in a series detailing my development environment and CI/CD deployment pipeline. [The code for the developer environment can be seen on my GitLab](https://gitlab.com/ryan-l-robinson/drupal-dev-environment). Other posts in the series can be seen by checking the Drupal Docker series links in the sidebar. I provided [an overview in the first post of the series](/2025/drupal-docker-deploys-overview/).

This series has now covered all the core functionality of having a local development environment and deploying updates, and most elements of testing.

This is done with a series of the "single use" jobs that I mentioned in the deployment post.

## Dump the Production Database

Dumping the production database in CI then importing it to dev and staging, to keep it up to date.

## Download the File for Local

Another job will simply show you the newest database backup file from the shared storage, allowing you to download it as an artifact. This lets you keep it as a backup in case anything goes wrong, like just before a deployment, as well as to update the content on the local developer environment.

## Import to Dev or Staging

Another job will allow running on dev or staging to import that database there.
