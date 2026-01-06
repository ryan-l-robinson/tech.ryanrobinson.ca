---
title: Eleventy Tag and Series Descriptions
date: 2026-01-11T13:16:00.000Z
author: Ryan Robinson
description: "Tags aren't always obvious what they mean, so I incorporated some longer descriptions for those tags to help."
series: "11ty Theme"
tags:
  - Static Sites
  - JavaScript
  - Accessibility
draft: true
---
This is a more subtle touch, but I quickly found myself wanting it once some of the other pieces were in place. It isn't always obvious what a tag means. Similarly, when I write a series of posts, I might want to repeat the same introduction on all of them, in order to avoid having to type basically the same thing each time, or worse, forgetting the introduction on some posts.

Fortunately, this isn't too hard to do with eleventy, utilizing the data directory to store the descriptions and then inserting them into the templates where desired.

## The Data Files

I have the descriptions set up as files in the _data directory, as a simple JSON syntax like:

```json
{
  "AI":
    "Machine learning tools: what are they good for, what are they not, and what are some of the social consequences.",
  "Accessibility":
    "Tech should be for everybody. Let's make that better."
}
```

There's one called tagDescriptions.json for tag descriptions and one called called seriesDescriptions.json for series descriptions. That makes sense, I hope.

## Add Series Descriptions

At least so far, the series description gets added in one place: the post template. This will put the description at the start of a post, underneath the metadata but before the post-specific content. You can see it in action on this very post.

This is what the relevant section of the post.njk layout looks like, showing the metadata header, the series description, and then the main content:

```nunjucks
  {% if seriesDescriptions[series] %}<p>Series Description: {{ seriesDescriptions[series] }} You can navigate other posts in the series using the list of links in the sidebar.</p>{% endif %}

  {{ content | safe }}
```

## Add Tag Descriptions

The tag descriptions got added in a couple of different ways, but the idea is similar.

On each tag's page, the description is shown at the top.

Then there are three places where the tag description is added as the title attribute on the link to the tag, so somebody hovering over the tag can get more details:

1. On a list of posts.
2. On an individual post.
3. In the sidebar list of all tags and the page of all tags (it's the same template).

Here's where those changes happened, starting with the list of posts in the post-list.njk template in 11ty-theme:

```nunjucks
{% if post.data.tags | filterTagList | length %}
  <p>Tags:
    {%- for tag in post.data.tags | filterTagList %}
      {%- set tagUrl %}/tags/{{ tag | slugify }}/{% endset %}
      <a href="{{ tagUrl }}" class="post-list-tag"{% if tagDescriptions[tag] %} title="{{ tagDescriptions[tag] }}"{% endif %}>{{ tag }}</a>{%- if not loop.last %}, {% endif %}
    {%- endfor %}
  </p>
{% endif %}
```

On the individual post in the post.njk layout, it looks similar:

```nunjucks
  <div class="post-metadata border-bottom-section">
    <p><time datetime="{{ page.date | htmlDateString }}">{{ page.date | readableDate("LLLL d, yyyy") }}</time></p>
    {% if tags | filterTagList | length %}
      <p>Tags:
      {%- for tag in tags | filterTagList %}
        {%- set tagUrl %}/tags/{{ tag | slugify }}/{% endset %}
        <a href="{{ tagUrl }}" class="post-tag"{% if tagDescriptions[tag] %} title="{{ tagDescriptions[tag] }}"{% endif %}>{{ tag }}</a>{%- if not loop.last %}, {% endif %}
      {%- endfor %}
      </p>
    {% endif %}
  </div>
```

Finally, in the tags-list.njk:

```nunjucks
{% for tag in collections | getKeys | filterTagList %}
  {% set tagUrl %}/tags/{{ tag | slugify }}/{% endset %}
  <li><a href="{{ tagUrl }}" class="post-tag"{% if tagDescriptions[tag] %} title="{{ tagDescriptions[tag] }}"{% endif %}>{{ tag }}</a> ({{ collections[tag].length }})</li>
{% endfor %}
```
