---
title: Eleventy Filtered Post Lists
date: 2026-01-07T16:40:00.000Z
author: Ryan Robinson
description: "How to create lists of posts that are filtered to certain tags or by year of the post."
series: "11ty Theme"
tags:
  - Static Sites
  - JavaScript
draft: true
---
The first challenge in building these eleventy sites was handling generating other post lists based on some filtering variable, particularly by tag and by year. This allowed me to build pages that are just all the posts tagged as [Accessibility](/tags/accessibility), for example, or all the posts published in [2026](/2026).

## The Post List Template

The post-list.njk template in 11ty-theme handles the looping and generating of each post:

```nunjucks
{# Loop for a list of posts, passed in as postList. #}
{%- css %}.post-list { counter-reset: start-from {{ (postListCounter or postList.length) + 1 }} }{% endcss %}
{%- css %}{% include "11ty-theme/css/post-list.css" %}{% endcss %}

{# Posts #}
{% for post in postList %}
  <article class="border-bottom-section">
    <h2 class="post-list-item-title"><a href="{{ post.url }}" class="post-list-item-link">{% if post.data.title %}{{ post.data.title }}{% else %}<code>{{ post.url }}</code>{% endif %}</a></h2>
    {% if post.data.description %}<p class="post-list-item-description">{{ post.data.description }}</p>{% endif %}
  </article>
{% endfor %}
```

## Filters

To be able to get all posts but with a certain tag or by a certain year, we'll need some filters and collections added to the theme's config.js file:

```js

```

## Generating Tag Pages

Tag pages, from each individual site under content/tag-page.njk, can now be generated.

```nunjucks
---
pagination:
  data: collections.tagPages.pages
  size: 1
  alias: tagPage
permalink: /tags/{{ tagPage.key | slugify }}/{% if tagPage.pageNumber > 0 %}{{ tagPage.pageNumber + 1 }}/{% endif %}
eleventyComputed:
  title: 'Tagged "{{ tagPage.key }}"{% if tagPage.pageNumber > 0 %} (Page {{ tagPage.pageNumber + 1 }}){% endif %}'
---
{% if tagDescriptions[tagPage.key] %}
  <p class="tag-description">{{ tagDescriptions[tagPage.key] }}</p>
{% endif %}

{% set postList = tagPage.posts %}
{% set pagination = tagPage %}
{% include "post-list-pager.njk" %}
```

## Year Pages

Year pages are similar to tag pages, but in content/year-page.njk:

```nunjucks
---
pagination:
  data: collections.postsByYear.pages
  size: 1
  alias: yearPage
permalink: /{{ yearPage.key }}/{% if yearPage.pageNumber > 0 %}{{ yearPage.pageNumber + 1 }}/{% endif %}
eleventyComputed:
  title: 'Posts from {{ yearPage.key }}{% if yearPage.pageNumber > 0 %} (Page {{ yearPage.pageNumber + 1 }}){% endif %}'
---
<p>Looking for all posts published in {{ yearPage.key }}? You've come to the right place.</p>
{% set postList = yearPage.posts %}
{% set pagination = yearPage %}
{% include "post-list-pager.njk" %}
```

## One More Note About Year

With the current URL structure, I have year pages that are simply the year, which is always in the format of a 4 digit number. I also have pages on the homepage which are also simply numbers, incrementing upward starting at 2 (1 is the homepage so that doesn't show up in the URL). That means that in theory, if I ever reach 20201 posts - 2020 pages of 10 posts each, plus one more to create page 2021 - the URL for the pagination of 2021 will be the same as the URL for the year 2021. In theory I don't like this and I may still change it later, but in practice it will never be a problem. I am not going to write enough other posts, possibly ever but definitely before doing a site redesign anyway.
