---
title: "CiviCRM: Personal Campaign Pages"
date: "2021-06-21T10:28:00-04:00"
author: "Ryan Robinson"
tags:
  - CRM
  - PHP
---

Have you ever taken part in a non-profit fundraising event where you get your own personal webpage that you can direct people to as a way for them to sponsor you directly in the event? Personally I am happy to participate in [the Coldest Night of the Year](https://cnoy.org/home) every year, which does have this type of system (not in CiviCRM).

In the language of [CiviCRM](/posts/2021/civicrm-overview/), these are Personal Campaign Pages. While CiviCRM does offer this functionality, it has some major holes in it. Like a few areas of CiviCRM, it feels like somebody threw it together quickly and then mostly forgot about it. Consequently, I had a hard time ever recommending it as a good solution, even if a client was already heavily using CiviCRM happily. [Here’s the official documentation](https://docs.civicrm.org/user/en/latest/contributions/personal-campaign-pages/).

Here are the three biggest problems I found when trying to use this feature:

## Confusing to Set Up

The way that CiviCRM presents options to you is confusing. Personal campaign pages can be used to recruit new participants or to gather donations. They can also be attached to either an event or a contribution page. So there are four combinations:

- On an event, using campaign pages to recruit new participants
- On an event, using campaign pages for participants to fundraise (the most common I’ve seen, like the CNOY setup)
- On a contribution page, using campaign pages to recruit participants (this scenario makes the least sense to me)
- On a contribution page, using campaign pages to fundraise

Maybe having all four options is justifiable. The bigger problem is the settings don’t make it clear enough which combination you want until you try it out and realize you got it wrong.

## No Participant Directory

The most natural thing that you want in a scenario like Coldest Night of the Year? A list of all the participants so that visitors to the site can find who they want to support, as well as things like leaderboards. This is common in these fundraising systems. The CiviCRM PCPs do not offer this.

If your CiviCRM is built on top of a Drupal site (the best option), you can make something work using views to find all the PCPs of participants tied to an event. But that shouldn’t be necessary; that really should be a feature that’s part of the CiviCRM offering. We put in a lot of effort with views and custom PHP development to try to make some friendly user interfaces for participants and donors and even then it was a little rough compared to other systems out there.

## No Teams

A lot of these types of events go most smoothly when the participants can sign up as part of a team, which adds elements of comradery and competition. The CiviCRM function does not handle this at all. There was an extension that did it, but that extension got abandoned after a couple of years, leaving one of our clients stranded without that working at all anymore and data in their database with no easy way to convert it to something useful. When it was working, it was similarly as rough as our participant directory workarounds.
