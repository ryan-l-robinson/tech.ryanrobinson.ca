---
title: "Shrinking Pagination with JavaScript"
description: "A JavaScript solution for pagination having less options on smaller screens."
date: 2025-12-19T16:40:00.000Z
author: Ryan Robinson
tags:
  - Drupal
  - JavaScript
---

## The Problem

On larger screens, I like having a nice big pager with several links, including specifying First, Previous, Next, and Last, with some numbers in between.

!["Full desktop pager, with links to First, Previous, 1, 2, 3, 4, 5, 6, 7, 8, 9, Next, and Last (19)"](./FullPager.png)

The problem with that is that it is not going to fit on smaller screens, and start wrapping into a second row which is hard to make look good. So instead, I came up with some JavaScript to help steadily remove items from the pager as required in order to fit, or more accurately, to start at the minimum and build up only with what will fit.

!["Shrunk mobile pager, with links to Previous, 5, and Next"](./SmallPager.png)

## The Solution Code

In my context, this was for a Drupal site, so I set this up with the JavaScript file being added in my custom public theme, and then added as a library that only gets loaded in the contexts that have pagers (views). I have not yet written up using the Drupal libraries system and I really should, because it's a pretty valuable thing to understand.

Here's the JavaScript file:

```js
/**
 * @file
 * Keep Drupal pagers on one line and dynamically show as many numeric
 * neighbors around the current page as will fit (symmetrically).
 *
 * Behavior:
 *  - Prev (.page-item-prev) and Next (.page-item-next) are always visible (if present).
 *  - Current number (.page-item-number.active) is always visible.
 *  - Grow numbers symmetrically around current (left+right), stopping right
 *    before items would wrap to a second line.
 *  - After numbers are maximized, try adding First + Last together. If only one
 *    exists (e.g., Last on page 1), try adding that single if it fits.
 *
 * Implementation:
 *  - Strictly measures wrap inside the <ul class="pagination js-pager__items">.
 *  - Drupal behavior attaches after initial load and after Ajax pager updates.
 *  - ResizeObserver + MutationObserver keep it accurate on resize and DOM swaps.
 *
 * Requirements:
 *  - Included in CSS that is also loaded: .page-item.is-hidden { display: none; }
 *  - Attach this JS as a library on templates where the pager appears.
 */
(function (Drupal, once) {
  'use strict';

  const HIDE_CLASS = 'is-hidden';

  const SELECTORS = {
    container: '.pager',
    row: '.pagination.js-pager__items, .pagination',
    item: 'li.page-item',
    first: 'li.page-item-first',
    last:  'li.page-item-last',
    prev:  'li.page-item-prev',
    next:  'li.page-item-next',
    number: 'li.page-item-number',
    current: 'li.page-item-number.active, li.page-item-number [aria-current="page"]',
  };

  // Utilities

  const qOne = (el, sel) => el.querySelector(sel);
  const qAll = (el, sel) => Array.from(el.querySelectorAll(sel));
  const addHide = (el) => el && el.classList.add(HIDE_CLASS);
  const removeHide = (el) => el && el.classList.remove(HIDE_CLASS);

  function getRow(pager) {
    return qOne(pager, SELECTORS.row) || pager;
  }

  // If any visible item has different offsetTop, that means it has wrapped and should be reduced.
  function isWrapped(row) {
    const items = qAll(row, `${SELECTORS.item}:not(.${HIDE_CLASS})`);
    if (items.length <= 1) return false;
    const top0 = items[0].offsetTop;
    for (let i = 1; i < items.length; i++) {
      if (items[i].offsetTop !== top0) {
        return true;
      }
    }
    return false;
  }

  function resetAll(pager) {
    qAll(pager, `.${HIDE_CLASS}`).forEach(removeHide);
  }

  function getParts(pager) {
    const row   = getRow(pager);
    const first = qOne(row, SELECTORS.first);
    const last  = qOne(row, SELECTORS.last);
    const prev  = qOne(row, SELECTORS.prev);
    const next  = qOne(row, SELECTORS.next);
    const numbers = qAll(row, SELECTORS.number);

    // Current: prefer the <li> with .active; fallback to aria-current inside.
    let currentEl = qOne(row, 'li.page-item-number.active') ||
                    (qOne(row, 'li.page-item-number a[aria-current="page"]')?.closest('li.page-item-number'));
    let currentIndex = numbers.indexOf(currentEl);
    if (currentIndex === -1 && numbers.length) {
      currentIndex = Math.floor(numbers.length / 2);
      currentEl = numbers[currentIndex] || null;
    }

    return { row, first, last, prev, next, numbers, currentEl, currentIndex };
  }

  function ensurePrevNextVisible(parts) {
    removeHide(parts.prev);
    removeHide(parts.next);
  }

  function hideAllNumbersExceptCurrent(parts) {
    const { numbers, currentEl } = parts;
    numbers.forEach((n) => (n === currentEl ? removeHide(n) : addHide(n)));
  }

  // Grow outward from current as long as it doesn't wrap.
  function growNumbers(parts) {
    const { row, numbers, currentIndex } = parts;
    if (!numbers.length || currentIndex < 0) return;

    let L = currentIndex - 1;
    let R = currentIndex + 1;
    let safety = numbers.length * 3;

    while (safety-- > 0) {
      const left  = L >= 0 ? numbers[L] : null;
      const right = R < numbers.length ? numbers[R] : null;
      if (!left && !right) break;

      if (left && right) {
        removeHide(left);
        removeHide(right);
        if (isWrapped(row)) {
          addHide(left);
          addHide(right);
          break;
        }
        L--; R++;
      } else {
        const one = left || right;
        removeHide(one);
        if (isWrapped(row)) {
          addHide(one);
          break;
        }
        if (one === left) L--; else R++;
      }
    }
  }

  // Add First + Last if they fit. If both exist, treat as a pair; if only one exists, try that one.
  function tryAddEnds(parts) {
    const { row, first, last } = parts;

    if (first && last) {
      addHide(first); addHide(last);
      removeHide(first); removeHide(last);
      if (isWrapped(row)) {
        addHide(first); addHide(last);
      }
      return;
    }

    const single = first || last;
    if (single) {
      addHide(single);
      removeHide(single);
      if (isWrapped(row)) {
        addHide(single);
      }
    }
  }

  function updatePager(pager) {
    if (!pager || pager.__destroyed) return;

    resetAll(pager);

    const parts = getParts(pager);
    if (!parts.currentEl) {
      return;
    }

    // Prev/Next must remain visible.
    ensurePrevNextVisible(parts);

    // Start minimal around current (hide First/Last and all other numbers).
    addHide(parts.first);
    addHide(parts.last);
    hideAllNumbersExceptCurrent(parts);

    // Grow symmetric numbers until just before wrapping.
    growNumbers(parts);

    // Try to add First/Last (pair if both exist, else single).
    tryAddEnds(parts);

    // Re-assert Prev/Next visibility (belt-and-suspenders).
    ensurePrevNextVisible(parts);
  }

  function attachObservers(pager) {
    // Debounce to once-per-frame.
    let rafId = 0;
    const schedule = () => {
      if (rafId) cancelAnimationFrame(rafId);
      rafId = requestAnimationFrame(() => updatePager(pager));
    };

    // Observe the <ul class="pagination js-pager__items"> row for size and DOM changes.
    const row = getRow(pager);

    const ro = new ResizeObserver(schedule);
    ro.observe(row);
    pager.__pagerRO = ro;

    const mo = new MutationObserver(schedule);
    mo.observe(row, {
      childList: true,
      subtree: true,
      attributes: true,
      attributeFilter: ['class', 'style'],
    });
    pager.__pagerMO = mo;

    // Fonts can reflow width so re-check once loaded.
    if (document.fonts && document.fonts.ready) {
      document.fonts.ready.then(schedule).catch(() => {});
    }

    // Wait until layout has settled.
    schedule();
  }

  function detachObservers(pager) {
    pager.__destroyed = true;
    if (pager.__pagerRO) { pager.__pagerRO.disconnect(); pager.__pagerRO = null; }
    if (pager.__pagerMO) { pager.__pagerMO.disconnect(); pager.__pagerMO = null; }
  }

  Drupal.behaviors.pagerOneLine = {
    attach(context) {
      // Keep the original token 'pager' to avoid attaching to the element as both the old token name and a changed name.
      const pagers = once('pager', SELECTORS.container, context);
      pagers.forEach((pager) => {
        attachObservers(pager);

        // Also re-check after clicks (useful for Ajax pager swaps).
        pager.addEventListener('click', (e) => {
          if (e.target.closest('a')) {
            setTimeout(() => updatePager(pager), 0);
          }
        });
      });
    },
    detach(context, settings, trigger) {
      if (trigger !== 'unload') return;
      qAll(context, SELECTORS.container).forEach(detachObservers);
    },
  };

})(Drupal, once);
```
