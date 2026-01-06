---
title: Eleventy Theme Updated
date: 2026-01-06T16:40:00.000Z
author: Ryan Robinson
description: "I've completed some major theme updates for this site. This post is an introduction, with more details in later posts."
series: "11ty Theme"
tags:
  - Static Sites
---

I also redid [the ryanrobinson.ca landing page](https://ryanrobinson.ca) with some similar design, but I'm not going to talk about that any more here - that's plain HTML and CSS and the only hard decisions was in what content I should put there.

## The Code Structure

For this post, I'm just going to note how the code is structured.

The code is split into two:

1. The theme that is shared between the two sites, housed at [the 11ty-theme project in my GitHub](https://github.com/ryan-l-robinson/11ty-theme).
2. The [individual site's project](https://github.com/ryan-l-robinson/tech.ryanrobinson.ca), which sets a lot of site configuration as well as contains all the post content.

The theme is not fully abstracted. There are some things in there which are uniquely intended for me, like all the social media links in the footer, but they are the same for the two sites so it didn't seem worth the effort to make the theme more abstract and then have to define all the links on each site separately. I could still do that if I learned that somebody else really wanted to use that theme, but that isn't likely so I can keep it more efficient and easier to maintain this way.

In the rest of this series, I'll look at some of the more confusing parts: pages listing posts by tag and by year, nested pagination, sidebar flexibility, tag and series descriptions, search, and maybe some more if I think of it later.
