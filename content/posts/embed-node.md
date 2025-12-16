---
title: Embedding Content in Other Content
date: 2025-08-20T22:54:16.700Z
author: Ryan Robinson
description: Sometimes you want to pull an Xhibit meme and put content in your content. Fortunately there's a module that helps.
tags:
  - Drupal
draft: true
---

In my work, I've encountered two scenarios where I want to embed some version of one node of content into another node. This has at least a couple significant benefits over copying the same text:

- If something needs updated on the source node, you won't have to update every other page referencing it separately.
- The same style can be enforced consistently so it always looks identical across multiple pages, instead of relying on different content authors to always use the same formatting.

Using the [entity_embed](https://www.drupal.org/project/entity_embed) module to allow ckeditor to put a view of another node within a node text field can solve this.

This module is fairly well documented and generally works as advertised, but I'll walk through the experience:

After the usual steps for a new module of installing it, go to the configuration screen.
