---
title: "Microsoft Teams: Lifecycle"
date: "2021-08-10T08:20:00-04:00"
author: "Ryan Robinson"
tags:
  - "Microsoft 365"
  - MS-700
  - Security
---

If you’ve been paying attention to Microsoft 365 products in the last 5 years or so, you’ve likely noticed that things have moved toward a much flatter architecture where users have more freedom to set up their own Teams / SharePoint sites, etc. In many ways this is great, but it does carry some risks of sprawl caused by users casually creating data structures and then forgetting about them.

Fortunately, Microsoft does offer some mechanisms in the Teams lifecycle to help with this.

## Templates

First up, when you’re creating a Team, a helpful tool to create them properly is templates. Some are available by default plus you can create your own. These help you establish if you have common needs for a Team, e.g. if you have a Team for every website project you can start that Team with a Web Project template that provides the exact channels and apps you need on website projects. This doesn’t stop the sprawl of creating too many groups, but it does help ensure that when groups are created, they are created properly and consistently.

## Team Creation Policies

By default every user in the organization can create Groups and Teams. That can be restricted, though. Especially in larger organizations, you may want to restrict to only a select few others outside the IT team who can create Teams. This can be done by creating a security group in Azure AD and granting them and only them the permission to create groups.

## Expiration Policies

One helpful tool to follow up on any groups that may have been created and never used is the expiration policy. The policy checks to make sure the group is still being used every x number of days. If it is, it will automatically renew. If it is not, it will email the group owner asking them if the group needs to be renewed or not. If the owner does not renew the group, it will be deleted.

## Naming Policies

If you have a lot of teams, you want to name them consistently. Group naming policies can be used to add prefixes or suffixes to group names. For the example of project teams, maybe you want every team to start or end with the word Project.

## Archive, Restore, Delete

Archive is another option for a team that mostly shuts down the Team, but keeps all the content available in a read-only format. It’s a good way to say that we’re done with this Team, but we might still need to reference it in the future, or keep it for legal compliance reasons.

If a team is archived, it can be restored back to the fully active mode.

Deleting is the permanent action. You will have 30 days that an admin can recover the group, but otherwise, that’s it. In general, you probably should archive groups that had significant content in them, while delete is only used for cleaning up accidental creations or obsolete groups where all the relevant content has been moved elsewhere.
