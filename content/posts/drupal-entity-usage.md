---
title: "Drupal: Entity Usage Tracking"
date: 2025-08-25T00:16:00.000Z
author: Ryan Robinson
description: Drupal's entity_usage module is quite useful.
tags:
  - Drupal
draft: true
---

[entity_module](https://www.drupal.org/project/entity_usage) is a very useful module that I came across in my work recently.

## The Problem

It initially came out of the entity embed scenario that I wrote about previously. I designed the use of "reusable content" so that when there was repetitive content to be put into multiple pages, the editor could create just one and embed it within the other pages. Then if something changed within that content, they would only need to update it once and it would affect all of them.

It left one problem, though: when you're editing a node of reusable content, how do you know what pages you're going to change? Even if nobody else was also embedding that reusable content somehow, do you really remember everywhere that relies on it? Probably not.

## The Solution

I began looking into building my own solution to this problem, would probably would have looked something like a view on each reusable content page to show any other pages referencing it. Then I quickly entity_usage which explicitly supports the entity embed scenario I had, plus some more.

It also solves for some other scenarios. If you try to delete a page, when there was another page linking to it, you know have a way to know that. You can decide whether to go change that other page first, or realize that you actually need to keep the page that you were about to delete.
