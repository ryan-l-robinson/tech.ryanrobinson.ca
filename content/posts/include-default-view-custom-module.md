---
title: "Drupal: Include Default View in Module"
date: 2023-02-23T22:46:00.000Z
author: Ryan Robinson
description: "How to include a view configuration as part of installing a module."
tags:
  - Drupal
  - PHP
---

You may be working on a Drupal module and want to include a default view.

## Install

This is actually quite simple: put the configuration file in the config/install folder within the module, with the proper configuration name, and it will install.

This would be the full configuration file, the same as you would generate using the configuration sync tools.
You can write this file from scratch, especially if they're hidden options. If it is something that has a user interface, you can do what I tend to do: determine all your preferred settings and test it out within the interface, then export the configuration from either the UI (/admin/config/development/configuration/single/export) or drush (`drush config-export` but that does all configuration, not only the specific one you want).

## Uninstall

What about uninstalling the configuration you've installed with the module? This will depend on the context of your module, but you may want to also uninstall this configuration when the module is uninstalled. The configuration might no longer make sense without the module, or you might create conflicts if you installed it once, uninstalled the module but left the configuration, then tried to install it again.

To do that, add a few lines like this to the uninstall hook of the .module file, for example with a config file named webform.webform.event_registration:

```php
/**
 * Implements hook_uninstall
 * */
function module_name_uninstall() {
  if (!empty(\Drupal::configFactory()->getEditable('webform.webform.event_registration'))) {
    \Drupal::configFactory()->getEditable('webform.webform.event_registration')->delete();
  }
}
```
