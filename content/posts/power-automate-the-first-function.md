---
title: "Power Automate: The First Function"
date: "2021-06-02T13:41:00-04:00"
author: "Ryan Robinson"
tags:
    - "Microsoft 365"
---

A scenario I’ve encountered several times in Power Automate is needing to get just one item from a data source, such as a SharePoint list, based on a specific column such as Title matching what I am looking for. Power Automate only has a function to get all the SharePoint items that match the criteria, unless you already know the specific ID you are looking for. So you end up with an array returned, even if there’s only one item in it. That then creates a bit of nuisance when you want to access that one item, since Power Automate will go ahead and put that within a for each loop structure. Knowing there’s only one item, it is a negligible difference in Flow speed, but it is suboptimal code looking at a for each loop that isn’t really looping.

How do you get around that? The Microsoft Tech Community recently answered that with the first() function. The full details are in the link below:

[Avoid Unnecessary Looping (Apply to each) in Power Automate – Microsoft Tech Community](https://techcommunity.microsoft.com/t5/microsoft-365-pnp-blog/avoid-unnecessary-looping-apply-to-each-in-power-automate/ba-p/2190265?WT.mc_id=m365-24198-cxa&utm_source=collab365&utm_medium=collab365today&utm_campaign=daily_digest)
