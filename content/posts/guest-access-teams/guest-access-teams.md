---
title: "Microsoft Teams: Guest Access"
date: "2021-08-13T08:31:00-04:00"
author: "Ryan Robinson"
description: "An overview of guest access in Teams."
series: MS-700
tags:
  - "Microsoft 365"
---

Microsoft Teams is not just for users in your organization. You can also use it to collaborate with people from other organizations or no organization at all. Here are a few things to keep in mind:

## Chat: External and Guest

The “chat” app in Microsoft Teams allows you to message users one-to-one or in small groups. This includes users in your organizations, but (if the organization allows it) also includes being able to message users with Teams in other organizations, in Skype for Business for other organizations, and in Skype consumer.

It’s the Teams users in other organizations that have one of the more confusing issues with Teams: external vs guest. Guests are users in other organizations that you have invited into your organization’s Azure AD, most likely to join a Team or SharePoint site to share resources. They would be signed in to their account but on your organization’s resources. External users are users in other organizations, signed in to their usual tenant.

Where it gets confusing is when somebody has become a guest in your organization. If you try to start a chat with them, you’ll see options to message them either as a guest or an external. Unfortunately, the Teams client only allows you to receive notifications from one tenant at a time. That user has to be either signed in to their tenant (which would make them external to you) or your tenant (which would make them a guest to you) at any given time, not both. You may need to have some preliminary conversations to know which one they expect you to message them on.

## Guest Users

The bigger question is adding guests to the resources within your tenant, to use within Teams as well as possibly elsewhere.

Depending on what you want guests to be able to use, you need to enable guest access in a few Microsoft 365 workloads:

- Azure AD to add guest users to groups
- SharePoint Admin Centre to allow sharing files with guests
- Teams Admin Centre to allow adding guests to teams

Anybody with permissions to do so can add guest users to a team. The user accepting the invitation is not always intuitive if they do not already have some familiarity with Microsoft 365, so you’ll want to have some documentation ready if you’re going to do this often.

## Guest permissions in Teams

The [Teams Admin Centre](https://admin.teams.microsoft.com) has plenty of options to configure exactly what you want guests to be able to do within the categories of calling, meeting, and messaging. You may want them to be able to do virtually everything an internal user can do, or you may want to significantly limit them to certain functions. It all depends on the relationship with your partner and the nature of the work you need to share.

!["Guest policy options"](./guest-policies.png)

There are also a couple smaller options that can be changed per team for whether guests can create, update, and delete channels in that team.

## Access reviews

[Azure AD access reviews](https://docs.microsoft.com/en-us/azure/active-directory/governance/access-reviews-overview) are not only about guest access, but they are a tool in Azure AD for prompt group owners regularly to make sure they don’t have anybody in their groups who don’t still need to be there. Having extra users in a group are not quite as big of a deal with internal users. Internal users might be able to see it anyway and are more likely to remember to remove themselves from a group. Guest users are more likely to have been forgotten about because of a project a couple of years ago. Access reviews can be a good way to catch those.
