---
title: "SharePoint: Hub to Hub Association"
date: 2021-12-16T12:15:00.000Z
author: Ryan Robinson
tags:
  - Microsoft 365
---

A long-awaited feature has arrived in SharePoint: [the ability to associate one hub site with another hub site](https://docs.microsoft.com/en-gb/SharePoint/hub-to-hub-association). Over a year ago I saw references to a PowerShell cmdlet to do this in the official documentation pages, but with a note that it was in limited preview so most of us wouldn’t be able to do it yet. Now it has finally rolled out to general availability.

## Why Do This

Associating one hub with another hub changes one thing: search results. If you’re in one hub and do a search, results from other associated hubs can be prioritized alongside other sites in the same hub. This is a nice extra level of controlling your search results for a pretty common scenario.

For example, imagine being in a small team with lots of projects. You want a site for every project, associated to your team’s hub site (for the search results, for a sites web part, etc). But your team is also a part of a larger department and you may have a lot of useful general resources on that site. Now when you search from your team’s SharePoint you can get results in both directions: the projects in the same hub and the general documents of the associated department hub.

## What It Doesn’t Do

It does not change any of the other things that come with the first level of hub association, most of which makes sense if you think about them.

Navigation doesn’t change. There is a shared hub menu. Nothing from the associated hub’s navigation will appear. But it makes sense that it doesn’t really work to have two conflicting navigation areas.

The automatic application of site designs for all sites in a hub also stays as only sites in that hub. This makes sense for the same reason navigation does: you can’t really have two site designs applied to the same site at once.

The sites web part, with the setting to filter by hub associations, doesn’t change. This one I think could make sense to expand, for scenarios much like the search: it could have one filter setting for only sites in this hub and another filter for sites in associated hubs as well. I may be missing something for why that couldn’t work, so maybe that feature will come later.

## More Details

Check out [SharePoint Maven’s recent post for more details](https://sharepointmaven.com/how-to-add-a-hub-to-another-hub-in-sharepoint-online/).
