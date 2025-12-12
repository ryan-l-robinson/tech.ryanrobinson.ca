---
title: Site Rebuilt in Eleventy
date: 2024-11-24T19:04:42.150Z
author: Ryan Robinson
description: "This site has been revamped with eleventy. This post describes some of my experiences with it."
tags:
  - Static Sites
  - CSS
  - Accessibility
---

I have rebuilt this site in [Eleventy](https://www.11ty.dev/), replacing the previous Jekyll version. Here are some of my thoughts on the experience:

## Advantages

It's got a couple advantages over my Jekyll experience.

One that eleventy tends to brag about is that it builds very fast. This doesn't matter that much to me in the context of a simple site mostly hosting blogs on GitHub Pages, but it is nice to at least know when I commit something that it will be visible for testing quickly.

The bigger one for me has been that I understand it more. I got the hang of the templating system pretty quickly, as its nunjucks is not far off from the twig I use for Drupal. With the old Jekyll site, it mostly felt like a black box that I didn't have enough control over. There probably was a way, but it wasn't obvious. Here I know what files are contributing to the markup, what files are contributing to the styles, and what extra JavaScript is running.

## Search

Search is one of the more significant disadvantages. There is no default search module. I implemented [PageFind](https://pagefind.app/). This mostly works well enough, and wasn't that hard to figure out, but unlike the big advantage noted above with eleventy templates, there isn't a huge amount of control over the UI of the end result. The largest problem I have with that is I am unsure it is the most accessible possible, e.g. it does not create [an aria-live region](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-live) when the content visible on the page has been changed, so screen reader users may not even know the new content is being displayed.

## Taxonomy Pages

The other big-ish technical piece that I have not solved yet is having pagination on the term pages like [the one for accessibility](/tags/accessibility/). This is because pagination is itself used to create those taxonomy pages programmatically, rather than needing to set up each tag's page manually, and there is no support for another level of pagination within those.

This isn't a huge problem for me at the moment, since my most-populated tag is at 49 posts and that isn't unreasonable to show on one page. But it could become a more serious problem eventually.

## Post tl;dr

Because there is relatively easy flexibility of what metadata you track for posts and how those appear in templates, I could do things like add a post tl;dr that shows as the summary on the posts lists, as well as the top of the individual post display. This is intended to be a quick summary when you visit a page, to understand if this is going to answer your question or whether you should look somewhere else. That came out of some consultation with a screen reader user, who expressed how it can be hard to get to a page and have to listen to the whole thing only to realize that she is on the wrong page.

## Series

I created another taxonomy for the series of a post. Now, if a post is part of a series, that whole series will appear in the sidebar of that post, making it easier to jump to other posts that you might need to read first or want to read next.

## Post Changes

A lot of the metadata in my posts needed to change to support this new structure instead of Jekyll's, including:

- Add a description for each post to serve as the tldr.
- Remove the category field which is no longer relevant.
- Change the filename to what I want the permalink to be, and remove the permalink from the front matter. It is possible to set the permalink through front matter to override the file name, but at least for now I've gone with simplicity.
- Update any links and image references, since those are all in new locations now.

## Domain

I have also changed the domain. Instead of ryanrobinson.technology, I'm using a subdomain, tech.ryanrobinson.ca. This is primarily an effort to simplify my web presence around the one main domain that hosts a broad landing page about me. Now that I have a better idea of how to build a site with eleventy, and have a theme I'm mostly happy with (some improvements noted below notwithstanding), I will likely make more subdomains off of ryanrobinson.ca for other types of blog content.

## Still To Do

While I am declaring this good enough to phase out the old site and get back to some writing, there are a few things I hope to still do as of writing this post:

- As mentioned above, I don't think the search functionality is as accessible as it can be. I may have to make a copy of the PageFind JavaScript UI in order to rework some of it, while keeping PageFind's handling of other aspects like building the index.
- The styles are not all great. I think they're readable, but there are things like the black of the dark mode probably still being too black.
- I have done some passes to review the posts, but it might not all be there yet. I need to review all the descriptions, tags, and series.
