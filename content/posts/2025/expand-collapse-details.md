---
title: Expanding and Collapsing Details Elements
date: 2025-11-28T17:36:16.700Z
author: Ryan Robinson
description: Details elements help organize content in a page, but some users need to expand them all at once to skim the page. Here's a Drupal module to do that.
tags:
    - Drupal
---

## The Problem with Collapsed Details

Details elements, commonly referred to as accordions, are a very valuable tool for tucking away some content into sections when not always needed, especially on pages that would be very long with lots of content that would take a while to scroll down if they were all expanded at once. However, they do have some problems as well, namely that it's easy to miss things that are hidden inside details elements. You can't use Ctrl+F, or other utilities that search everything on the page, to find them.

## The Solution

A solution is to include buttons on the page, before the details elements, that when pressed will expand all the details elements. One click, and now you can use Ctrl+F or get a better overview of how much content you're dealing with again!

I have a [Drupal module for this available in my GitHub](https://github.com/ryan-l-robinson/drupal-module-expand-collapse-details).

Here are some of the key components of that code:

### The Block

The block itself looks like this, as a class in the src/Plugin/Block folder:

```php
<?php

namespace Drupal\expand_details\Plugin\Block;

use Drupal\Core\Block\BlockBase;
use Drupal\Core\Cache\Cache;
use Drupal\Core\Render\Markup;

/**
 * Displays button for expanding or collapsing accordions.
 *
 * @Block(
 *   id = "expand_details_block",
 *   admin_label = @Translation("Expand and Collapse Details Buttons"),
 *   category = @Translation("Expand Details Module"),
 * )
 */
class ExpandDetailsBlock extends BlockBase {

  /**
   * {@inheritdoc}
   */
  public function build(): array {
    return [
      '#markup' => Markup::create('
        <button type="button" name="Expand" aria-label="Expand all content on page" class="expand-details-button expand-details-button-expand">
          Expand All +
        </button>
        <button type="button" name="Collapse" aria-label="Collapse all content on page" class="expand-details-button expand-details-button-collapse">
          Collapse All -
        </button>
      '),
      '#attached' => [
        'library' => [
          'expand_details/expand_details',
        ],
      ],
      '#cache' => [
        'contexts' => ['theme'],
        'tags' => [],
        'max-age' => Cache::PERMANENT,
      ],
    ];
  }

}
```

If you haven't built a custom block before, a couple pieces of this are essential:

1. That location in the file structure, src/Plugin/Block and a filename that matches the class name, so it can be properly recognized as a block plugin.
2. The comment block with those keys, which specify a machine name and how it will show up in the Block Layout interface.

Otherwise, the build function determines the markup of that block. Along with the markup itself, it also declares:

1. There is an attached library, which will connect to the next important file. This is how JavaScript and CSS are added to the block.
2. The cache is straightforward, only altering by theme and never clearing automatically. This is because there is nothing here that would change by page or by user or any other contextual variation so it can stay cached and be highly efficient.

### The Library

The module's .libraries.yml file declares any libraries for the module, which in this context mean a bundle of CSS and/or JavaScript that can be applied to only certain contexts. In this module, there is just one library and we already saw that it would only be added to the context of that block, so this CSS and this JavaScript doesn't need to be loaded on any other page.

```yml
expand_details:
  version: 1.1
  js:
    js/expand_details.js: {}
  css:
    theme:
      css/expand_details.css: {}
```

One more note here is the version number. When you are working with JavaScript, increment the version number after changes. If you don't, you might not see your changes reflected and you'll think it isn't working. Clearing the site's caches will not resolve this. You need to update the library number so your browser knows it is different JavaScript. That's not a problem with CSS, where clearing the site's caches will reflect the update immediately whether or not the version number was increased.

### The JavaScript

This is the core functional part, the JavaScript:

```js
(function () {
    // Get details elements, excluding some special cases.
    var detailsElements = document.querySelectorAll('details:not(.building-hours):not(.captcha):not(.bef--secondary');
    // Get blocks, which could be more than once.
    var expandDetailsBlocks = document.querySelectorAll('.block-expand-details-block');

    if (detailsElements.length > 0 && expandDetailsBlocks.length > 0) {
        expandDetailsBlocks.forEach((expandDetailsBlock) => {
            // Display change is set at the block level.
            expandDetailsBlock.style.display = 'flex';

            // Assuming that there will only ever be one of each per block.
            var expandDetailsButton = expandDetailsBlock.querySelector('.expand-details-button-expand');
            var collapseDetailsButton = expandDetailsBlock.querySelector('.expand-details-button-collapse');

            // Add click actions for each block.
            expandDetailsButton.addEventListener("click", () => {
                detailsElements.forEach((details) => {
                    details.open = true;
                })
            });
            collapseDetailsButton.addEventListener("click", () => {
                detailsElements.forEach((details) => {
                    details.open = false;
                })
            });
        });
    }

})();
```

There are three exclusions added in this example: building hours which is a little details dropdown to show open hours in the header, any appearance of the captcha from the captcha module, and [the Better Exposed Filters advanced filter](https://www.drupal.org/project/better_exposed_filters). You may have more or less things that you want to exclude. A potential bit of improvement for this module would be to get those options into configuration so they can be more easily adjusted, but I have not done that here.

## The Styles

Finally, the style sheet:

```css
/** Hidden by default, it will only appear when there are details on the page **/
.block-expand-details-block {
  display: none;
  flex-direction: row;
  justify-content: flex-end;
  margin-bottom: 0.5em;
}

.expand-details-button {
  border: 2px solid black;
  border-radius: 5px;
  background-color: gold;
  color: black;
}

.expand-details-button:is(:hover, :focus) {
  background-color: purple;
  color: white;
}
```

The first part is the most important here: by default, it does not display. It only appears once the JavaScript detects that there are details elements (other than the exclusions). It would be confusing if it showed up even when there are no details elements on the page.

The other rules are just giving it some style.
