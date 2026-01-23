---
title: Text Underline Position
date: 2024-02-21T15:22:00.000Z
author: Ryan Robinson
description: "Use text-underline-position under for improved readability of underlined links."
tags:
  - CSS
  - Accessibility
---

This one is a quick accessibility CSS tip. I have long been a proponent of the old standard of underlining links, and never underlining anything else. It helps make it clear that it is a link. Add a hover and focus effect to make it more obvious when they are selecting it, too.

But it can introduce a problem: for certain reading disabilities, the line being close up against the text can make it hard to read. Even for myself without one of those disabilities, once it was flagged for me I immediately saw it every time I looked at a link.

So here's a quick CSS solution that significantly helps.

Use `text-underline-position: under` to make links with underlines easier to read. It moves the underline down to the bottom of the hanging letters (g, y, j) rather than right up tight against the bottom of other letters and cutting right through the middle of the hanging letters. It may not quite be perfect - even better might be a few pixels below the hanging letters to get space there as well - but it solves the majority of the problem with one quick CSS change.
