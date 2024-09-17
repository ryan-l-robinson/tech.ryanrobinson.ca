---
title: "SharePoint: Overriding a Site&#8217;s Home Page"
date: "2021-06-14T13:26:00-04:00"
author: "Ryan Robinson"
tags:
  - "Microsoft 365"
  - "SharePoint Site Provisioning"
---

This post continues a series on [SharePoint site provisioning](/tags/sharepoint-site-provisioning/), unpacking some of the problems I’ve faced and overcome in building SharePoint site provisioning solutions.

At this point in the series, we’ve now created new SharePoint sites and applied temporary site scripts and site designs. There is one big feature missing from site scripts and site designs, though: templating the home page. You can’t simply say that a project site design should always contain a home page with these specific web parts. This may not be true forever – Microsoft is steadily improving site templating – but as of working on this project a few months ago, it required a bit of a workaround.

## The Idea

Here’s the basic idea that we’ll do to get around this:

1. Design your homepage on some other SharePoint site, with permissions locked down so that nobody sees it or can edit it who shouldn’t.
2. Within the provisioning Flow, after the new site is created, copy the page from the template site to the new site, overriding the name of “Home” so that it takes over the homepage.

## The Pro and Con

The nicest thing about this approach is that it makes it very easy to update the home page. Anybody with the permission to do so can go to that template site’s home page and edit it. That’s it. There is no PowerShell or JSON to update a site script or design.

The biggest negative is the other side of that coin: it requires that nobody delete or rename that homepage file on that template site and there’s no warning if somebody did try to do that. You can recover within 93 days if somebody does delete it and you notice by way of the Flow failing, but it’s still not ideal that it’s even possible for a user to do that without knowing the consequences.

## The Copy Action

If you don’t already have a template site with some locked down permissions, create it and then design the homepage. This homepage can use relative web parts, like the documents library of that site.

When that’s ready, it only takes a “Copy file” SharePoint action in the Flow to handle the copying. This can be a bit smoother than some previous actions which required the HTTP REST API calls. Those needed more flexibility using variables defined in the Flow. This one is copying from the same location every time, so it can stay simpler.

![](/assets/img/2021/06/CopyHomepage.png)

Details:

- Current site address: select your template site
- File to copy: /SitePages/Home.aspx
- Destination site address: the SiteURL variable used previously, for the full URL of the new site
- Destination folder: /SitePages
- If another file is already there: Replace

Note that this action depends on the new site already being fully created. [If you followed my previous post](/microsoft-365/power-automate/temporary-site-scripts-and-designs/), there was a delay there for the purpose of applying the site design. If you don’t already have a delay between the request to create the site and copying this file, you’ll need to add one.
