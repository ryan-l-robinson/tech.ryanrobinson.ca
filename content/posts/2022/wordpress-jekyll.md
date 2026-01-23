---
title: WordPress vs Jekyll
date: 2022-05-10T15:22:00.000Z
author: Ryan Robinson
description: "A breakdown of having a WordPress site vs a Jekyll site."
tags:
  - WordPress
  - Static Sites
---

I recently switched from running this blog off a WordPress site to running it off a GitHub Pages site built on a Jekyll theme. Here's a few thoughts on the pro's and con's of each.

## Users

WordPress offers support for multiple users with different permissions. You can have some admins with full site control, some editors to oversee content, some authors who can only write new content.

Jekyll does not, except inasmuch as you have multiple contributors to the GitHub project. You can set a different author name for a post in the front matter, but there's no permission level for ongoing content management.

## Ease of Building

Somebody inexperienced with website building can still have a pretty good WordPress ssite ready in half an hour, especially on WordPress.com but even on many self-hosted WordPress sites since many hosts provide tools to quickly install it in a few clicks.

Jekyll takes more expertise. Even when using an existing theme, you still need to learn a few things about GitHub, edit configuration in files (not interfaces), and handle tasks like resizing image assets and cleaning up code yourself.

## Cost

GitHub Pages is [free as in cats](/websites/civicrm/free-as-in-cats/). WordPress will usually cost at least a little in hosting, unless you do the free ad-supported WordPress.com instead, but that has other limitations.

## Maintenance

With a WordPress self-hosted site you need to update WordPress, plugins, and server software like PHP on a regular basis or you'll have exposed security vulnerabilities. It's not hard most of the time - unless you hit a situation like your theme not working with secure versions of PHP - but it is something you need to remember.

There's none of that with a Jekyll GitHub Pages site. Just don't delete your GitHub project. There is no PHP, no server maintenance, no juggling of a dozen contributed modules that may or may not be maintained.

## Formatting

WordPress offers a lot of built-in editing and formatting tools, well beyond the basics of HTML tags. It has a lot of little features that add up. A couple of my favourites include searching for other pages on your site to add links, which means you seldom end up with a bad link, and an image upload interface paired with resizing tools to always show at the right size.

With Jekyll the sites are written in markdown. It's a bit faster if you know what you're doing but has a lesser set of design options, such as no columns. Depending on your editor, you may also need to memorize the markdown syntax for each format you want, without a WYSIWYG. It also doesn't have those other helpful checks like confirming that a link really does exist on the site before you publish it.

So which is faster? Depends on the complexity of your posts. If you're mostly writing text with some headers, like this post, markdown can be pretty quick and easy on any device. But if you want columns, easy images, links to other pages on your site... WordPress may end up faster.

## More Features

WordPress also includes other features like:

1. Scheduling to publish at a certain time in the future - useful to optimize your release window for maximum engagement
2. Post to your social media when it does publish
3. Formulaically generate the permalink for the page, with options to define that formula

Jekyll doesn't offer any of that. That does mean more overhead to do these things manually.
