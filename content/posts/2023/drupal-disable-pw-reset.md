---
title: "Drupal: Disable Password Reset"
date: 2023-01-08T17:00:00-05:00
author: Ryan Robinson
description: "How to block the user registration and password reset forms in Drupal."
tags:
  - Drupal
  - Security
---

In one particular site, we chose to block the user registration and user password reset options. The site is using SSO login anyway, so this would really only impact our development team's superuser admin account (user 1) that is typically blocked outside of an emergency, so this seals up a possible attack vector with no user impact.

User registration can also be blocked with site configuration, so I wouldn't have bothered with custom code for only that part. But since I was doing it for the password reset anyway, I decided to go ahead with the extra layer of protection on the registration route as well.

I achieved this with a simple custom module which you can view in full [in my GitHub](https://github.com/ryan-l-robinson/Drupal-disable-password-reset). Here's the key part, blocking access to the routes:

```php
  protected function alterRoutes(RouteCollection $collection) {
    // Always deny access to unwanted routes.
    $disallow_routes = [
      'user.register',
      'user.pass',
    ];
    foreach ($disallow_routes as $disallow_route) {
      if ($route = $collection->get($disallow_route)) {
        $route->setRequirement('_access', 'FALSE');
      }
    }
  }
```
