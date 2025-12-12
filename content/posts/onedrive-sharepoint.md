---
title: "OneDrive vs SharePoint"
date: "2021-04-21T02:59:00-04:00"
author: "Ryan Robinson"
tags:
  - Microsoft 365
---

The first question that typically comes up when moving files to Microsoft 365 is this: what’s the difference between OneDrive and SharePoint? Which files should I put where?

## Permissions

The most important difference is the default permissions. **In short, files that are for just you should be in OneDrive. Files that are for others should be in SharePoint.** OneDrive is individual by default. SharePoint is shared by default.

This includes files where you are doing most/all of the work but others might need to access them at some point in the future. You want to minimize friction if somebody else on a team suddenly needs to access that file. At best when the file was put initially in your OneDrive, they need to request that you share it, which could have been avoided by putting it in a shared location in the first place. At worst, what if you aren’t available? They might not be able to get to the file they need right now, or might need to take up the time of an administrator to step in.

There is a mechanism for admins to give one user access to another user’s OneDrive. This can be helpful in circumstances like a sudden termination where it turns out the terminated employee had some files that others need. But at most this should be used for times like a new employee taking over the same role as a previous user, where there are files that are only used by that one role.

SharePoint sites are most easily set up attached to Microsoft 365 Groups. Those Groups already carry permissions across all the services of Microsoft 365 like Planner and Teams. If you devise some logical groups, your files can neatly into those groups. Users don’t have to think about permissions for each file; they just need to put it in the relevant group.

This also extends to search. When files are in SharePoint, anybody with the permissions to see it will also be able to find it within [Microsoft Search](/microsoft-365/microsoft-search-introduction/), unless search indexing is turned off for that SharePoint site.

## Columns, Views, and Content Types

SharePoint brings a pile of functionality that isn’t available in OneDrive. These include:

- Columns, for tracking custom data about the file
- Views, for presenting the files in different ways such as sorting, filtering, and grouping the results
- Content types, for enforcing certain types across multiple document libraries, including possibly with file templates
- Power Automate processes shared with others
- Power Apps forms shared with others

If your workflow benefits from anything more complicated than just a few people able to see the same file, these extra tools can make a huge difference. This lines up nicely with the permissions aspect, since those functions are also generally most useful when the file is for multiple users.
