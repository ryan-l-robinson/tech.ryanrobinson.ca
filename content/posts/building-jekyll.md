---
title: "GitHub Pages: Building a Jekyll Site"
date: 2022-05-05T11:39:18.000Z
author: Ryan Robinson
description: "An overview of building a Jekyll site hosted on GitHub Pages."
tags:
    - Git, GitLab, and GitHub
    - Static Sites
---

I recently decided that I wanted to try out hosting this site on GitHub Pages instead of WordPress that I had been using for over a decade. The reason why is simple: it’s [free as in cats](/posts/2022/free-as-in-cats/) and it’s easier to maintain after the initial work of setting it up, with no regular maintenance required.

Here’s a high level look at how I did it, and the code [is available in my GitHub](https://github.com/ryan-l-robinson/ryan-l-robinson.github.io). The main branch is everything I control, then the GitHub Actions generate the site on the gh-pages branch.

## Chirpy Theme

I looked around for a bit and chose to start from [the Chirpy theme](http://jekyllthemes.org/themes/jekyll-theme-chirpy/). I like its simple design while still supporting the key blog features: categories, tags, archive timeline. And the biggest selling point was the switch between dark and light modes. That’s a nice feature to offer visitors.

It does have a few things I wish were better. The navigation sidebar is not in a nav element, so that's a bit less than ideal accessibility. It also leans heavily on using ID for every element instead of class, which means my [Visual Studio Code](/tags/visual-studio-code/) is constantly warning me that this is a fragile way to build a site. And the default colour contrasts are not strong enough in several places, but that can be overridden with CSS (see below).

Knowing myself, I'll probably try to find a different theme in 1-2 years, but for now, this will do.

## Config

The next step was to create the general config of the site and any pages outside of the blog posts like the About page. Config provided several options including where to find the profile photo, which social network links to show in the sidebar and as share buttons on posts, whether to default to light or dark mode.

About is a simple markdown file with some front matter, and placed within the \_tabs folder which will add it to the main navigation. Adding more pages to this \_tabs folder will add them to the main navigation as well.

## Assets

The largest piece of work was editing the assets, specifically the CSS stylesheet. The key idea for this is that anything I put in a folder called “assets” would add to and override the default assets folder on the site. This includes images and stylesheets. This stylesheet behaviour is similar to WordPress where you can have a child theme build on and override certain pieces of the stylesheet.

## Posts

I chose to use a WordPress plugin to export everything on my WordPress site into Jekyll format. It worked pretty well, but did leave a few pieces I wanted to clean up myself. Some of the front matter it included was not relevant to my theme, so I removed those. The image in the front matter was not the right syntax, so I had to adjust each of those. And it wrapped every image in a figure tag with a caption, while this theme supports a simpler syntax for images, so I also replaced those. Some of the code around images and YouTube video embeds were messier than they needed to be.

I'll have more in a future post about how I write and publish posts.

## Domain

What I have not done yet is assigning a domain. I will move my ryanrobinson.techology domain to here, but haven't yet. [It seems like a pretty straightforward process](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site).
