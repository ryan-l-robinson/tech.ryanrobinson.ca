---
title: Introduction to Drupal Caching
date: 2025-12-22T14:54:16.700Z
author: Ryan Robinson
description: "Caches are an important tool in Drupal to keep page loads as efficient as possible, without risking showing old inaccurate information."
tags:
  - Drupal
draft: true
---

Proper caching in Drupal development is one of those things that can be easily missed. This can result in problems in a few ways:

1. If caches don't clear enough, the page will show outdated information.
2. If they clear too often, that's extra page load time and extra processing needed for the server every time.
3. If cache contexts are not well defined, the wrong information could get cached and displayed where you don't want it.

Here are the big concepts as well as some examples.

## Contexts

The first big concept is the idea of a context. Contexts define what situations should be considered unique for a tag. A context may be one thing, or a combination of several things, including:

- The theme. If used on its own, this is saying that it is the same in all contexts where it is used on that theme.
- The current user, at the individual level or the permission role level.
- The current page, with some variations possible for the path or query arguments.

### Theme Context

If there's nothing variable about the template you wish to cache except which theme's CSS it uses, you can use the context of theme. This would be highly efficient, since it almost never needs to regenerate from scratch, just when you manually clear all caches or you change the theme. But of course that's the simplest possible version.

### User Context

The content of the section may change based on which user, if it is something like a block that shows the current user's event registration history. In that case, you will need to set the context by individual user. That means it is going to be a little slower for the first time each user loads it (and every time after all caches were cleared), but will be faster after that.

### Role Context

The content may also change based on the role of the user. Perhaps admins have an extra link that others don't, so you need to separate that for most authenticated users it looks like one thing and for admin users it looks like another thing, with the cache context making sure that the wrong one isn't displayed to a user of the wrong role.

### Page Context

Perhaps the content changes based on which page it is tied to. This may be something like view results that change based on the exposed filters parameters, or a block showing in the sidebar that shows other information about the node you're viewing.

## Tags

The next big concept is tags. The tags can be used to define under what conditions the cache should be rebuilt.

There are many default tags that are provided by Drupal core or by other modules. For example, the default node_list tag is for any time there is a change in any nodes on the site. If you have a view that is showing all nodes on it, that is a great tag to clear anytime that you want the view to update.

Remember the trade-off in efficiency, though. If you have a site with a lot of nodes being edited regularly, that's going to clear the cache for that view a lot. For this example of a view showing all nodes, that is probably the best option. Depending on the need, though, you might be able to get more precise: does it only need to rebuild caches on changes to certain content types, or to a specific node? If so, you can change the cache tags to something more precise that won't need to wipe out that cache more than necessary. The question is: what are the scenarios where the case needs to clear, and how can you achieve that without too much unnecessary clearing as well?

## Max Age

Finally, the max age defines a maximum amount of time in seconds before the cache will need to rebuild.

This also includes an option to be set to Persistent, or -1, which means the cache will stay until manually cleared or cleared by tag. If it can be clearly defined as only ever being associated with a tag being invalidated, it's highly efficient to set the max age to permanent and let the tags do all the work.

You can also set the max age to 0, meaning it is never cached. This probably isn't ideal performance usually, but if it is something that needs to represent frequent changes and those causes of the changes are too complicated to easily put into tags, it may be a better option than trying to building a lot of custom code you need to maintain just to handle a tiny efficiency improvement.

## Example: Custom Sidebar Block

Here's one example: I want custom blocks that show content related to the current node being displayed, in order to show that data in a sidebar block.

In this case, I want to combine a few things:

1. Set the context to url, in order to clarify that each URL should have its own caches version, not the same cached version across other pages.
2. Set the tag to the tag of the node.
3. Set the max age to persistent.

Combined, this is saying that the cache for the block will clear if and only if the node is updated. That's exactly the optimal performance scenario, with no chance of showing bad information, for this example.

There's one more complication, though: what if the block is empty? That's a little different. When the block is empty, it actually needs to not cache at all, i.e. set the max-age to 0.

Code example:

```php

```

## Example: View with Custom Tag Override and Timed Filters

This one is a bit more complicated, but is a pretty common scenario: what do I do if I have a few like "upcoming events"? I want to show all nodes of type event, but only when they're still in the future.

The first part is a little more obvious. Let's narrow down the tags on the view. It only shows content of type event, so it doesn't need to be rebuilding for every change to every type of node, which is the default view setting. This could be done programmatically, but if you're doing a lot of views, it's probably worth installing a module that helps.

TODO: get the name of that module again, add some screenshots, etc.

That helps with narrowing down the frequency of caches to only those changes that matter. It doesn't help with another aspect of this view, though: timing. Only upcoming events should be shown. Since the nodes aren't changing at all when the time is past, you can't rely on tags. You could use a short max age, but that's only efficient if you're updating events a lot anyway.

The other part is a little more tricky. How does it know to clear the cache on the view when one of the events is going to switch from upcoming to past? If it doesn't, it will keep something something as upcoming that it shouldn't, at least until the other cache clearing scenario of updating a different event.

Here's a workaround to set the max age using the difference in time between now and when the next event starts.

```php

```
