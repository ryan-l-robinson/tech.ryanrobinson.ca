---
title: "Drupal: CSS Structure"
date: 2022-09-19T21:00:00.000Z
author: Ryan Robinson
description: "An overview of how Drupal handles hierarchy of CSS."
tags:
  - Drupal
  - CSS
---

Specificity rules for CSS priority in Drupal are determined on two axes:

1. Active custom subthemes take priority over their parent theme.
2. [Stylesheets are split into priority groups as described here](https://www.drupal.org/docs/develop/standards/css/css-file-organization-for-drupal-9).

Most of the times the descriptions for each category will hold accurate. However, you may encounter, for example, that a parent theme or a module put something in "theme" when it really is better categorized as a "component." In the scenario that you need to override that style, you'll have to also put yours at the "theme" level or it will not take priority.

The end result may be something like this in the theme's *.libraries.yml file:

```yml
global-styling:
  version: 1.x
  js:
    js/navigation.js: {}
    js/search.js: {}
  css:
    base:
      css/base/elements.css: {}
      css/base/variables.css: {}
    layout:
      css/layout/containers.css: {}
      css/layout/footer.css: {}
      css/layout/header.css: {}
      css/layout/sidebar.css: {}
    component:
      css/components/buttons.css: {}
      css/components/details.css: {}
      css/components/field.css: {}
      css/components/forms.css: {}
      css/components/images.css: {}
      css/components/lists.css: {}
      css/components/tables.css: {}
      css/components/tabs.css: {}
      css/components/views.css: {}
    theme:
      css/theme/admin.toolbar.css: {}
      css/theme/search-results.css: {}
      //use.fontawesome.com/releases/v6.1.2/css/all.css: { type: external }
```

Note that theme in this example also includes an imported stylesheet from elsewhere, in this case fontawesome for the social media icons. This works great, but you may want to remember to periodically updated for new versions.
