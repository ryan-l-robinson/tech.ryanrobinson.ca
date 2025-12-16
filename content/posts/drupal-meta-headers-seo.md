---
title: Drupal Meta Headers
date: 2024-04-08T15:46:00.000Z
author: Ryan Robinson
description: You might want to change meta tags for SEO purposes. Here's how to do that with Drupal theme preprocessors.
tags:
  - Drupal
draft: true
---

## The Meta Tags

There are a few meta tags which you can put in your site to impact their search results:

- noindex: keep search engines from processing the page entirely. It won't show up in search results, at least for the major search engines that respect that tag.
- title: the title given to the search result.
- description: the description that appears under the title. This should be a maximum of 160 characters. If you don't provide one, Google/Bing/etc will show the first 160 characters, which may or may not be what you really want to show.

## How: Theme Preproccesors

Adding the meta tags for noindex, title, and description from the theme preprocessor, with some variations by content type to read different fields.

Suppose there's one content type that I don't want to ever be indexed, because they'll only show up through views, for example, rather than going to it directly:

```php
Sample here
```

Suppose there's a content type for events where I want to add "Event" to the start of the title to help make it clear that this is a time-limited event.

```php
Sample here
```

Let's stick with that event scenario. For an event, I may want to make sure the summary shown to social media includes the date of the event, as well as some text from a summary field.

```php
Sample here.
```

That relies on a helper function that cleans up any special characters and trims it to 160 characters with some ellipses afterward. Here's that function, which I used to sanitize the final result of multiple content types.

```php
Sample here.
```
