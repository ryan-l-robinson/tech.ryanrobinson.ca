---
title: Dark Content Mode
date: 2024-02-21T21:46:00.000Z
author: Ryan Robinson
description: "Get ready for browsers converting your site to dark content mode."
tags:
  - CSS
  - Accessibility
---

In case you haven't caught it yet, there's new functionality coming to browsers for dark mode applied to content. This could come as a surprise to web developers who did not plan for dark mode in their design, but is overall a win for those of us who prefer dark mode on all the things all the time.

It's already become my default on Android Edge, inheriting that I have my device in dark mode. I haven't found it to be default on any other browser (including desktop Edge). Chrome has it as an experiment features flag in chrome://flags, so I did most of my testing by enabling that.

## Issues Found

The biggest problem I found in my main work site was with images that were previously assuming white backgrounds. I found the nicest solution to that was using an SVG instead of a PNG file, which the dark content mode adapted to invert the colours quite nicely.

There were also some insufficient contrast I discovered, like grey borders around form text boxes, that were maybe a little weak in light mode but were barely visible in dark mode. Many of these were quickly solvable by making it a stronger contrast. For example, instead of a grey border against the white background, I made it into a black border. It content dark mode, it would flip to a white border on black background. Both modes came out looking better.

I identified a few other contrast elements that were not quite ideal. But attempting a similar approach did not work. There were a few variations where I had black against the default light theme and trying it with the dark content mode turned it into a grey instead of a white - far from invisible, but not as strong of a contrast as I wanted. This is a new feature, not enabled by default other than that Android Edge that few people use, so I'm hoping this gets cleaned up on the browser end and there isn't much else I need to do.

## No Way to Target?

I tried the media query for [prefers-color-scheme](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme) but it doesn't help for this. That detects if the device prefers dark mode, not if the browser's dark content mode is on. It's nice if you want to specifically define what everything should look like in dark vs light modes. But as far as I could find, there's no way to specifically target content dark mode at this time.
