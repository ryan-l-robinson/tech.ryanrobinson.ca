---
title: "Power Apps: People Lookup"
date: 2022-05-02T16:00:00.000Z
author: Ryan Robinson
description: "Adding a field for looking up people in the Microsoft 365 as part of a Power App."
series: SharePoint Site Provisioning
tags:
    - Microsoft 365
---

## Background

A while ago I started a post in the series on [SharePoint site provisioning](/tags/sharepoint-site-provisioning/), unpacking some of the problems I’ve faced and overcome in building SharePoint site provisioning solutions. Back then, it was much more complicated to add a user from your Azure Active Directory as a field stored on an item. It's trivial when you're using a SharePoint list as the back-end for the data storage, since there are people lookup fields in SharePoint lists. But it's more complicated if you want to put it into DataVerse or not store it anywhere - only send it directly to a Power Automate Flow.

The latter is what I wanted to do with the SharePoint site provisioning. I wanted to accept a user on the Power Apps form and then the Flow would assign permissions based on that. I did not successfully finish writing that up, so I'll only include my old incomplete notes below.

## Good News

I finish the post now because there's great news: [Microsoft has now made that much easier out of the box](https://powerapps.microsoft.com/en-us/blog/announcing-the-aad-user-virtual-table-find-and-add-any-aad-user-to-your-records/). That post includes lots of information for how to use this new tool.

## Old Way

Here's the video from the old way of doing it that I was starting to implement, but you can now safely ignore that and use the new way instead.

<iframe width="560" height="315" src="https://www.youtube.com/embed/xs_hWRNCwuA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
