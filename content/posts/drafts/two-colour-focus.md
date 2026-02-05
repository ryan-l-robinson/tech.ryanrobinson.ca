---
title: Two-Colour Focus Indicators
date: 2026-02-07T17:11:16.700Z
author: Ryan Robinson
description: "WCAG is now encouraging a certain focus indicator style, with two colours appearing outside the normal size of the item."
tags:
  - CSS
  - Accessibility
---
[The Web Content and Accessibility Guidelines has some documentation here](https://www.w3.org/WAI/WCAG22/Understanding/focus-appearance) that addresses making focus indicators more consistent and always easy to see.

One approach is to have the same focus style on the entire site. This might cause a problem in that the style will show up better in some contexts than in others, e.g. if it is a dark border, that won't show up as well in the menu with a dark background as it does in the white background main area.

Another approach is to have different styles depending on where it appears. This takes a bit more CSS work to define each possible combination, but on a lot of sites it is definitely doable. I have usually taken this approach. It does have a problem, too, though: it isn't as obvious that it is indicating focus when it is a different effect depending on where it appears. A user could start tabbing through the menu, seeing one effect, then get to the main content and suddenly it is a different effect. It might not be obvious that it is indicating the same thing - focus - because the user has started to think of one style as focus and now it is something else.

The solution is to have two colours, one light and one dark, across the entire site. That way, at least one of the two will stand out against any backdrop.

Here is what I have decided to implement on this site:

TODO: still need to test this better against light mode, and actually implement it on this site.

```css
:focus {
	border: 2px solid var(--blue);
	outline: 2px solid var(--white);
	outline-offset: 2px;
}
```
