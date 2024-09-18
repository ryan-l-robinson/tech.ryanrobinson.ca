---
title: "Drupal: Sync Content"
date: 2022-05-26T19:31:19.000Z
author: Ryan Robinson
tags:
  - Drupal
  - DevOps
---

I have [previously covered syncing configuration across Drupal](/posts/2022/drupal-sync-configuration/). But configuration is not everything in the database. The other half of the picture is the content: nodes, custom blocks, menu items, files, etc. 

There is [an extra module available called content_sync](https://www.drupal.org/project/content_sync). Unfortunately, it is not a very reliable extra module, at least in my configuration. With that said, I will still share the main extra consideration to take into your plans.

The caveat for this is that it gets a bit more complicated in most regular usage than configuration, because content changes can happen in both directions.

Some changes will happen in the same direction as code changes and most configuration changes, especially in the early stages of the project before production is launched: first it gets changed on the local development system, then pushed through the stages to get to production.

After a site is launched, often the changes in content are being made by regular users on the production site. Maybe in your context the only content editors are also the website administrators, in which case this isn’t a concern, but often you’ll need to navigate how to make sure you don’t get conflicts.

In our case, in the early stage I did have content changes going the same direction as configuration:

1. Add content on my local Docker container
2. Export the content
3. Push the content sync files through to GitLab, which continues the CI/CD processes to the next server
4. The CI/CD imports the content on the next server

It started to break down around the same time that we let beta testers onto the dev server. I did warn the beta testers that this is a playground server to try things and try to find bugs, not a server to finalize content, with it in mind that I would likely be overriding content with my usual daily changes. But content_sync completely stopped working around the same time, so I made the call that I would stop using it instead.

I am still loosely keeping an eye on that module's development. I would like to have it for the occasional manual sync requirements between servers.
