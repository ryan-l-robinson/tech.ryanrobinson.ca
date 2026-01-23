---
title: "Azure AD: Guest Users from Google"
date: "2021-05-06T07:29:00-04:00"
author: "Ryan Robinson"
description: "Microsoft 365 does allow guests logging in with Google accounts."
tags:
  - "Microsoft 365"
---

Most users of Microsoft 365 have encountered the idea of guest users at some point. If you want to share company resources to somebody outside the company, you can do that. By default this works with the guest able to sign in with a business Microsoft account (Office) or a consumer Microsoft account (MSA).

This is not on by default, but you can take that a step further and allow guest users to authenticate logging in to guest resources using their **Google** accounts. In many scenarios, this is helpful compared to the default. Especially if it’s something like dealing with volunteers, you don’t really want to force them to make a Microsoft account if they don’t otherwise have one. If they don’t have a Microsoft they probably have a Google. It can also help if you use Google Suite for your main company email but also want some other functionality with Office, so that you only need to login with the Google rather than have a whole new set of users.

I won’t repeat the full steps here. [Microsoft’s documentation covers that well](https://docs.microsoft.com/en-us/azure/active-directory/external-identities/google-federation). I just wanted to share that it was possible as it is a bit of a hidden gem that isn’t obvious when you get started in Microsoft 365.
