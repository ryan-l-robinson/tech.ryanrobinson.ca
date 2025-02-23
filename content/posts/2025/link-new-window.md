---
title: Opening Links in New Tabs
date: 2025-02-23T17:50:16.700Z
author: Ryan Robinson
description: Links can be coded to open in new tabs/windows. Here's why to usually not do that, what the exceptions are, and how to do that accessibly.
tags:
    - Drupal
    - Accessibility
---

Links can be set to open in new windows/tabs or (by default) not. You mostly shouldn't do that, but there are a few exceptions and a few ways to carry out those exceptions.

## Accessibility: When to Do It

The decision for whether to open links in a new tab is not a simple matter of personal preference. You need to think about the experience for all users, including those relying on assistive technology such as screen readers. Most of the time, don't open in a new tab. This is because it can be confusing in a screen reader to be directed into a new tab without noticing, then being unable to use features they're used to like a back button. That's bad, so mostly don't do it.

There is one exception category, however: if the user needs to reference the new tab while still doing something in the original tab. The most common scenario is a form that has a link to more information about something that might change you fill out the form. If you go to the extra information in the same tab, you lose all the work filling out the form so far, and there's no potential to go back and forth. In that case, a new tab is worthwhile.

## Accessibility: How to Do It

With that said, if you do need to do it, it needs to be as clear as possible (without being unnecessarily annoying) to all of the users.

### Verbal Indicator

It needs to be especially clear to those who cannot see that the new tab has been opened. To do this, make sure one way or another that it will be announced to the screen reader before they click. The easiest way to do that is to set the aria-label. For example, if the link's visible text said "privacy policy" then the aria-label would be "privacy policy, opens in new window." That might not be the most obvious to do within a content management system context, but often there is a way; if you're a developer and not just a content creator, it's your job to figure out how to offer that.

### Visual Indicator

While maybe not quite as urgent, similar to the point above, you do also want to notify sighted users that a new window is about to open. Sure, they can tell afterward because they can see the extra tab open, but it can still be an extra nuisance if it catches them off guard. So for that, I decided on a fairly common approach of adding an icon after any link that opens in new tabs.

Here's a quick bit of CSS to help show an icon for any link set to open in a new tab/window. I grabbed an icon from Font Awesome that is a pretty standard one for this purpose.

First, if you don't use it already, make sure the source of the icon is loaded in your site. In my case, I used [the all CSS path](https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.7.2/css/all.min.css). This link is for the current version 6.7.2. You may need to look up the latest version. You also might be happy with a smaller version for a subset of the icons; in my case I was using enough other ones in other contexts that it made sense to take it all.

Now you can add a style like this to the appropriate place in your stylesheet:

```css
a[target="_blank"]::after {
  display: inline-flex;
  margin-left: 5px;
  content: "\f08e";
  font-family: "FontAwesome";
}
```

The display value of inline-flex has a minor advantage in my case compared to the default. My links are normally underlined. Without that inline-flex distinction, the icon would also be underlined, but that margin in between the text and icon would not be, which looks awkward. By switching the display value, it removed the underline from the icon, looking cleaner.
