---
title: "Drupal: Security Audit"
date: 2022-09-19T23:19:19.000Z
author: Ryan Robinson
tags:
    - Drupal
    - Security
---

About a year ago, I (virtually) attended [the DrupalGovCon 2021 conference](https://www.drupalgovcon.org/). The highlight for me was the session on improving security of your Drupal website. I’ve embedded the video below and summarized some of the major points.

<iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" frameborder="0" height="337" loading="lazy" src="https://www.youtube.com/embed/ghoFlwrC3SM?list=PLsGrHy_lmfhT5cl0P_I737mo6952uIkt6" title="Tips for Securing Your Drupal Application" width="600"></iframe>

## Security Modules

Review settings of security modules including:

- [ ] jsonapi_extras
- [ ] flood_control
- [ ] login_security
- [ ] username_enumeration_prevention
- [ ] honeypot
- [ ] reCAPTCHA
- [ ] seckit

## Dev Modules

Ensure none of your development modules are on production:

- field_ui
- views_ui
- devel
- anything migration related
- stage_file_proxy

I don't have a perfect solution in place for this yet. I did try the config_split module for a time, allowing me to have separation with some development-related modules on our local development containers but not on production. Unfortunately it was becoming buggy.

## Config Cleanup

- [ ] Remove unused views
- [ ] Check that there is correct access control on all views
- [ ] Review permission roles, especially anonymous and authenticated
- [ ] Confirm that user registration is limited to admins
- [ ] Confirm that there are no test user accounts in production
- [ ] Confirm that file size upload limits are reasonable
- [ ] Confirm that private and public file directories are used properly

## Custom Modules and Themes

- [ ] Grep for any standard debugging phrases like var_dump which may still be left in your custom code
- [ ] Check code in an editor like VS Code that warns you of unsanitary code
- [ ] Look for any cases where you missed using the t function to make text translatable. Even if your site is entirely in one language, it's still good practice.
- [ ] Review any TODO / FIXME etc in the code

## Users

- [ ] Review users. Anybody, e.g. former staff, still have active accounts that they shouldn't? Does anybody have more permissions than they need?
- [ ] Confirm that the superuser account is blocked and that nobody else has the superuser role in production.
