---
title: "Microsoft Excel vs SharePoint Lists"
date: "2021-06-23T14:50:00-04:00"
author: "Ryan Robinson"
description: "A breakdown of when to use Excel and when to use SharePoint Lists."
tags:
  - "Microsoft 365"
---

I’ve been asked the question before: when should I use Microsoft Excel and when should I use SharePoint lists? At first I didn’t have a great answer, but I thought about it, looked into it a bit more, and this is what I settled on:

**Excel is better when you need to do operations on the whole data set. SharePoint lists are better when the list items stand more-or-less as independent objects.**

Let’s break down a bit of why I concluded that.

## Excel: Operations

Excel can do a lot of data crunching that SharePoint lists don’t on their own (maybe with Power BI, but let’s ignore that for now). You want to make an easy chart summarizing the data? That’s Excel. You want to sum a column? That’s Excel. You want to calculate different multipliers based on different variables, like a complex budget or quote spreadsheet? That’s Excel.

!["A simple Excel table doing sum calculations in both directions"](./excel-sums.png)

## SharePoint List: Editing

SharePoint lists are much easier to edit if you need to change one particular item. You can open up a form, either using the default SharePoint forms or a connected Power App, to edit that particular item. You can also edit that one through a stand-alone Power App, through a Flow in Power Apps, or through the mobile Lists app. Much more than with Excel, it is designed to see a single item on its own and make edits as necessary.

!["Form to edit a single item in SharePoint"](./edit-issue.png)

## SharePoint List: Sorting and Filtering

Excel can do some sorting and filtering, but it isn’t completely intuitive. It’s easy to accidentally end up sorting just one column instead of all the associated data in other columns, ruining the data (hit undo).

Sorting and filtering is much more intuitive in SharePoint lists. You’ve likely encountered the same kinds of interface on many other websites with rows of data. Just click on the heading to sort or bring up the filters menu. You won’t ever accidentally jumble your data because every row is a distinct object.

!["Options that appear when clicking on a column header, including sorting"](./column-settings.png)

## SharePoint List: Saved Views

Related to sorting and filtering, SharePoint makes it easy to save different views. These views can be simple tables but with different sorts and filters applied, or could be much more complicated with [list and column formatting](https://docs.microsoft.com/en-us/sharepoint/dev/declarative-customization/column-formatting). These views can be saved and shared with everybody else that uses the list, or could be kept private for just you. This is extremely valuable functionality.

!["Views menu including editing views and changing between existing views"](./views-menu.png)
