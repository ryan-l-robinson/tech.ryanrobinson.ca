---
title: "Security Essentials: Password Manager"
date: "2021-03-07T13:58:14-05:00"
author: "Ryan Robinson"
tags:
  - Security
---

I typically have two main pieces of advice for basic information security that anybody can and should do.

- Use multi-factor authentication everywhere it is offered
- Use a password manager to generate good passwords, remember them, and make your life easier with auto-fill

[My previous post dealt with multi-factor authentication](/posts/2021/security-essentials-multi-factor-authentication/). This post will look at password managers.

## Passwords are the Worst

Why do you need a password manager? The short answer: passwords are the worst, but with a few exceptions, we still need them for everything. There are a few problems with passwords:

- We need too many of them, so most people start reusing the same few passwords across multiple services. This opens a big security vulnerability: if the password for one service gets leaked, attackers will try the same username / password across lots of other services and get in to them all.
- It’s hard to think of a good one that is both sufficiently random to keep attackers from guessing it and memorable enough that you won’t forget it when you need it again a year later. This leads to people making simple passwords like “password” or again using the same one across multiple services.
- They’re annoying to type if you have all the random characters and symbols that are often recommended or required, especially in situations like a media app on a TV where you don’t get a full physical keyboard.
- These problems are all amplified if you share the account with other people, like your Netflix account that the entire family needs to access.
- They are a thin line of defense if they are the only thing keeping somebody out of your account. Even if the password is great, that’s still only one thing that anybody from anywhere could guess and break in.

The last one is dealt with by multi-factor authentication. Most of the others – other than the typing on TV scenario – are dealt with by a password manager.

## The Password Manager Solution

The gist of the idea of a password manager is that you only need to remember one password to get into your password manager account, and that account remembers all your other passwords. That solves the memory problem.

Most if not all password managers also include a feature that will generate random passwords for you, storing them to the password manager all in one step. That solves the picking unique strong passwords problem.

Most if not all password managers have browser extensions and auto-fill in apps on phones. You don’t have to look up the password and copy/paste it over. You just click on the right account from your password manager and it fills it in for you. You never even need to look at the password for the account you’re logging in to, let alone remembering it and typing it out. That solves the annoying to type out problem.

Most if not all password managers have some mechanism for sharing those passwords with others. If one person changes the password, everybody gets the update and will auto-fill with the new one without ever even knowing it was changed. That solves the shared accounts problem.

Most password managers come with some other tools that also help boost your password security:

- Identifies if you’ve used the same password on more than one service. If you already had repeated passwords, this helps you find them.
- Identifies which services offer multi-factor authentication, to help encourage you to enable it. Some even function as a code generator themselves and will auto-fill the code for you, although I personally prefer the extra work of having a separate authenticator app.
- Identifies if any credentials have been leaked, as found on [haveibeenpwned.com](https://haveibeenpwned.com/). If any credentials show up here, you need to change them immediately – on everywhere with that password, not just the one that leaked it.
- Identifies weak passwords that have low levels of randomness and could be easier for an attacker to guess or brute force.

## Getting Started

Most of the password managers have some level of free trial with limited functionality. That’s a great way to get a sense of what it is like to use one before you commit to spending on a particular service.

Personally, I am now using [1Password](https://1password.com/). It offers all of the things I have mentioned above, in a friendly user interface and a reasonable monthly price.

Before that I used the free version of LastPass. For a long time it had the best free offering by far, but that has been scaled back since.

Then I used Enpass, which functions a bit differently. Instead of everything sitting in their secure cloud that you pay for on a monthly basis, it instead sets up your password vaults in your existing cloud services like OneDrive or Dropbox and syncs through that. It’s nice in that there’s only an up-front fee, not a monthly one, but needing to set up separate vaults in separate cloud services did make for a hassle trying to share with others.

Take your time trying out some different services to find what fits your workflow best. But the most important thing is simply that you start using one regularly and take advantage of what it offers so you get unique, strong passwords that you no longer have to memorize. Plus, unlike multi-factor authentication that makes logging in a bit more complicated occasionally, a password manager is both more secure and saves you a lot of time and mental energy.
