---
description: Rule for general site information
author: Ryan Robinson
version: 1.0
globs: ["**/*"]
tags: ["coding-guideline"]
---

This site is built on the eleventy static site generator, then hosted on GitHub Pages with help from a GitHub Action.

It includes 11ty-theme as a git submodule, set up that way because it is shared across multiple projects. If something is general purpose and would be a relevant change to multiple sites, as much as possible the change should be put into the shared theme. If it is unique to a specific site, it should be placed elsewhere.
