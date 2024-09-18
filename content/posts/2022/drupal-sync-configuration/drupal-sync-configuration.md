---
title: "Drupal: Sync Configuration"
date: 2022-02-18T00:14:00.000Z
author: Ryan Robinson
tags:
  - Drupal
  - Git, GitLab, and GitHub
  - GitLab DevOps
---

Having a workflow that keeps your code in sync across development, staging, and production servers – like in the series of [GitLab DevOps](/tags/gitlab-devops/) posts I’ve been sharing recently – is important. But that doesn’t synchronize the database, which contains two major subcategories: configuration and content. It also doesn’t synchronize user-uploaded files, but that’s a subject for a different post.

Fortunately, Drupal 8 introduced a new system for syncing configuration across copies of the site.

## What’s Included

It is not a full database backup, but it does include most settings, such as:

- Enabled extensions and themes
- Theme configuration
- Views
- Blocks
- Module settings

What it does not include is node or entity content. With the possible exception of initially building the site, that shouldn’t have any meaningful deficit on completing the work.

This distinction between configuration and content makes sense in a couple ways.

Configuration changes are usually made first in your development environment, tested thoroughly, and then pushed through the pipeline. That’s not true for content. Content is often going to be created or edited first by the regular site users on production. Those don’t need technical teams to approve and test everything and are available to more users to do directly and have go into effect immediately.

You also don’t need to synchronize all content in the same way, since for the most part the presence or absence of an extra basic page node does not affect the ability to sufficiently test anything new that needs to be tested. It’s ok if you let your production site’s content get ahead of your development environment in a way that you don’t want to happen with configuration.

## Git Workflow

The ideal flow for these changes is integrated with your code git flow. You can export your configuration on dev, add it to the git repository, push through to the next server in your CI/CD chain, and import it there. Much of this can be streamlined with CI/CD tools, but here are the manual steps:

To export the configuration:

```bash
drush config-export
```

Add to the git repository and commit:

```bash
git add sync/config/*
git commit -m "Updated configuration"
git push
```

Switch over to the other server and do essentially the reverse:

```bash
git fetch
git pull
drush config-import
```

More on [the drush command](https://www.drush.org/latest/commands/config_import/) can be seen here.

## Manual

You can also sync config manually one group of settings at a time. This is a slower process, so you wouldn’t take this route in a normal workflow of pushing dev changes out to production.

![Configuration sync screen, allowing for import, export, and full archive or single items](./config-sync.png)

I won’t detail that here, since the interface does make it fairly intuitive, but you can approach it halfway – sync the files but then import them one at a time from the synchronize tab – or you can avoid syncing entirely and instead manually copy the configuration file shown on one Drupal site into the other.
