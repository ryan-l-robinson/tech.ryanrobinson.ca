---
title: "Security Essentials: Multi-factor Authentication"
date: "2021-03-07T17:33:08-05:00"
author: "Ryan Robinson"
tags:
  - Security
---

I typically have two main pieces of advice for basic information security that anybody can and should do.

- Use multi-factor authentication everywhere it is offered
- Use a password manager to generate good passwords, remember them, and make your life easier with auto-fill

I’ll look at the benefits of a password manager in another post soon. But first, let’s look at multi-factor authentication.

## The Idea

The idea of multi-factor authentication is that you prove you are who you say you are through an extra “factor” beyond just a password. This is because it is relatively easy for a password to be stolen, distributed, and used. If all it takes for somebody to get into your important accounts is a username and password, that’s a low barrier to cause a lot of damage, especially when you add that most people reuse the same passwords on multiple sites (more on that in the password manager post).

## Verification Options

The extra factor may be something you know like a security question or something you have like a fingerprint. [This Microsoft doc breaks down the options in terms of what is available within Azure AD authentication](https://docs.microsoft.com/en-gb/learn/modules/secure-aad-users-with-mfa/5-configure-authentication-methods), but most services offer some subset of those options.

At the lowest end of the security scale are security questions, which are essentially just an extra password except easier to guess. Many places including Microsoft don’t offer a security question option.

In the middle and the most common are SMS codes texted to your phone or voice calls made to your phone saying the code. That’s good enough for most people, but determined hackers can carry out [sim-jacking](https://www.vice.com/en/article/3kx4ej/sim-jacking-mobile-phone-fraud) to get through.

The highest end of the security scale is an authentication app on your phone (Microsoft Authenticator, Google Authenticator) or a dedicated security key device that must be plugged into your computer via USB ([YubiKey](https://www.yubico.com/products)). With these protections, a hacker would have to know your username and password AND also have your phone logged in to the authenticator app or the physical security key. That means they would have to physically rob you on top of cracking your password, which significantly cuts down on how many people can realistically pull it off. It also makes it likely you’ll notice before much damage is done.

## Using an Authenticator App

The authenticator apps may sound like more work than a simple text, but it really is easier on top of being more secure. I use Microsoft Authenticator and that’s what I will be referencing specifically, but Google Authenticator and others have similar if not identical functionality.

Adding an account is simple. The hardest part may be finding the setting to enable it on the desired account. Some services make it obvious and strongly encourage you to enable multi-factor authentication. Others offer it but tuck it away in settings that you may not ever notice without looking for it. But once you do, if there’s an option to use an authenticator app, choose that and you’ll get a QR code.

In the top right corner of Microsoft Authenticator is a menu option with “Add account.” It then offers you the choice of a Microsoft personal account, a Microsoft work or school account, or other. The Microsoft option will allow you to set it up with a simple login, but all of the options allow you to scan the QR code. A quick point of your phone camera at your screen and you’re good to go.

The next time you try to log in and get prompted for the multi-factor – and it won’t typically be every time you log in since most services don’t prompt in low-risk scenarios like the same browser on the same computer at the same IP address as a login yesterday – then there are a couple of ways to verify your identity.

- Microsoft accounts in Microsoft Authenticator will push a notification prompt and all you need to do is select the Approve button. The small risk in this scenario is that people get used to clicking Approve whenever it pops up and may do it without thinking twice when an attacker is trying to get in.
- Otherwise you’ll need to copy the 6 digit code generated in the app into the web browser or app you’re trying to log into. These codes recycle quickly, generating new ones every 30 seconds, so it is virtually impossible for an attacker to guess the right one in time.

That’s it! It’s true that it is more work than just a password, but very little. You won’t have to do it often, mostly just when you use a new computer or phone. In exchange, you get a level of security that stops approximately 99.9% of attacks before they even get into your account. You won’t regret taking that bit of extra time to set up multi-factor authentication, but you will absolutely will regret it if you don’t and somebody gets into your account.
