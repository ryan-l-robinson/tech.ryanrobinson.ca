---
title: "Drupal Dev Environment Docker Compose"
date: 2025-12-25T00:16:00.000Z
author: Ryan Robinson
description: "Docker Compose for the Drupal setup, including a two-tiered approach for differentiating local."
series: Drupal Docker
tags:
  - Drupal
  - DevOps
draft: true
---
This post is one of several in a series detailing my development environment and CI/CD deployment pipeline. [The code for the developer environment can be seen on my GitLab](https://gitlab.com/ryan-l-robinson/drupal-dev-environment). Other posts in the series can be seen by checking the Drupal Docker series links in the sidebar. I provided [an overview in the first post of the series](/2025/drupal-docker-deploys-overview/).

We now have a robust Dockerfile for the main web image. There are other services needed, though, and there are some variations needed between local development and the other environments.

## The Two-Tier Setup


