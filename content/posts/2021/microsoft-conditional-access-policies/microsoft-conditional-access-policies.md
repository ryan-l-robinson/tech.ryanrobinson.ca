---
title: "Azure AD: Conditional Access Policies"
date: "2021-05-10T08:24:00-04:00"
author: "Ryan Robinson"
description: "An overview of conditional access policies."
series: MS-101
tags:
  - "Microsoft 365"
  - Security
---

Passwords are inadequate. Even for standard consumer tools, you should have at least two more tools in your toolbox: [a password manager](/posts/2021/security-essentials-password-manager/) and [multi-factor authentication](/posts/2021/security-essentials-multi-factor-authentication/). Those help make passwords suck less. But they do leave open some questions like: should you need to perform multi-factor authentication every time you log in? Should access be all or nothing, or should there be any accounting for degrees of risk?

Conditional access policies, part of Microsoft Azure AD, goes a step further. It’s built on a [zero trust model](https://www.microsoft.com/en-ca/security/business/zero-trust), meaning that it starts by assuming that you shouldn’t have access and then builds up trust based on several factors. Those factors usually include a password (except for [the new passwordless option](https://www.microsoft.com/en-ca/security/business/identity-access-management/passwordless-authentication)), but are not limited to them, in order to help cover over all the problems that come with passwords. Some of these other conditions include:

- **Sign-in risk**: a tool in Azure Identity Protection that evaluates risk on this particular sign-in, which includes variables like signing in from an anonymous IP address
- **User risk**: also part of Azure Identity Protection that evaluates risk based on the user’s behaviour over time
- **Device platform**: what kind of device are they trying to sign in on
- **Location**: based on IP address, e.g. your office
- **Device state**: paired with device compliance policies from Endpoint Management, this can set access rules based on whether the device is compliant or not

!["Conditional Access Policies options"](./conditional-access-policies.png)

Conditional access policies allow you to define exactly what you want to happen depending on a variety of risk factors. Along with those conditions, you get options to configure:

- What users or groups this policy applies to. For example, you probably want stricter conditions for admin accounts.
- What cloud apps or actions are affected. For example, perhaps in a low user risk scenario, you want to block access to SharePoint but not email.
- Whether you want to block or grant access. Grant access will allow signing in, but only after some extra security checks, like multi-factor or device compliance. Imagine a scenario like a user taking their work laptop to a coffee shop (use the location for the condition). You may want to allow that but require MFA again. The extra MFA check helps protect against the possibility that the laptop was stolen but it still lets the real user in.
- Session restrictions which can require signing in more frequently. In that coffee shop example, you may want to require signing in again every hour to limit potential that the user left their machine open to go to the bathroom.
- Whether the policy is on, off, or report-only. The report-only option can be helpful as a trial run to see how many people meet the conditions over a week before starting to enforce the restrictions. You may discover a lot more people work in coffee shops than you realized and that could alter your strategy of how to best enforce security.
