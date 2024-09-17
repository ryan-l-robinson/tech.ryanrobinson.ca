---
title: "Drupal: Active Links in Views"
date: 2022-12-30T01:46:00.000Z
author: Ryan Robinson
tags:
  - Drupal
---

Drupal 9 provides a different style for active links - i.e. a link that goes to the same page that you're already on - in most contexts. This helps identify for users when it would be redundant to select it again. There's one exception where it doesn't do this out of the box, however: within views.

Here's how I got around that. First add this file to the theme's /js folder, in a new file called `active-links.js`.

```js
(function (Drupal) {
  var viewLinks = document.querySelectorAll('.view a[href="'+window.location.pathname+'"]');
  for (let i = 0; i < viewLinks.length; i++) {
    viewLinks[i].classList.add("is-active");
  }
})(Drupal);
```

This is a simple function, checking to see if there are any links within views that point to the same address as the current page, and if it finds any, it adds the standard `is-active` class to it.

As with any other new JavaScript or CSS file, you'll also need to tell Drupal to include this library if you want it to show up. Adding it to the global-styling for the theme will load it on every page. You can do that by adding a new line to the libraries.yml file, within the js section under global-styling:

```yml
global-styling:
  js:
    js/active-links.js: {}
```

This could also be structured as a module instead of dropping it into the theme. That would make it more easily sharable for others to include it on their sites instead of needing to adapt it to their themes. But it is also a very small piece of code and that may be more hassle than its worth, so I am not currently planning on doing that.

Note: you will probably need to clear caches, close your browser, and open a new one, before you'll notice the change in place. JavaScript will be cached so you might be tempted to think it isn't working when it really just needs to reload.