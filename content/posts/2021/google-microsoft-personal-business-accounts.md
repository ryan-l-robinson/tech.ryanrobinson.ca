---
title: "Google vs Microsoft Personal and Business Accounts"
date: "2021-07-05T10:05:00-04:00"
author: "Ryan Robinson"
description: "Google and Microsoft handle things differently if you have both a personal and business account, each of which have some pro's and con's."
tags:
  - "Microsoft 365"
---

I recently started doing an email migration into Google Workspace. As I was creating the users, I encountered a wrinkle: one of the email addresses already had a personal Google account associated with it. Google gave me two options:

1. Notify the user of the personal account, prompting them to allow the account to be claimed by the organization.
2. Create a different user.

There was no option to allow the email to be associated with both a personal and a business account at the same time.

This is a different approach than Microsoft. Microsoft does allow you to have the same email address on both a personal account and a business account. This made me think about the pro’s and con’s of both approaches.

## The Problem with Google’s Approach

This new situation did remind me of hitting a similar situation when I got my first Android device. I had a Google account, but it was technically a business account, created years ago when GSuite was free. I initially set up my Android with that. Then I started hitting some limitations, like not being able to use family sharing of Google Play purchases.

So I was stuck with three options:

1. Keep using the business account and lose that functionality.
2. Use a different Gmail for Android, and consequently other services like Google Assistant. I did have one; I just didn’t use it for anything except the occasional testing of email tools.
3. Delete my business Google account and create a new personal one, losing all the data that was previously associated with it, which wasn’t a lot but did have a few things like YouTube subscriptions and history.

I ultimately went with number 3. It took longer. Yes, I did lose my YouTube history, although that might have been a blessing in disguise as YouTube’s horrible algorithm had been pushing a lot of terrible videos at me and it seems to have been better since. I got to keep logging in with my primary email.

I do still have one smaller problem with it. I have a Google account that is the same email as my primary Microsoft account. My calendar is on the Microsoft account. I would love for Google Assistant to see the calendar, but it can’t read the Microsoft calendar directly and I can’t share from Microsoft to Google because that is normally possible by sending an email and in this case that would just send the invite right back to the Microsoft mailbox sending it. Being able to put an alias on a Google account like you can a Microsoft would solve it, but Google doesn’t offer that.

It will be a much bigger problem if I ever move that domain to Google business email. Aliases would also make this easier. With this particular domain, that is unlikely. But if I was advising somebody else on this problem, I would suggest number 2. Just use a separate email @gmail.com that is explicitly for all things Google. Don’t try to be cute with your custom domain, or you could end up in a very bad situation. It would be nice if Google warned you about this possibility when you sign up with a different domain (maybe it does in small print I missed).

That situation demonstrates the advantage of Microsoft’s approach: **you can choose to keep your personal content and your business content separate** even if you intentionally or accidentally used the same email address on both. Plus Microsoft offers aliases which can help you untangle them if you later need to; I would be a lot more sympathetic to Google’s approach if they offered that.

## The Problem with Microsoft’s Approach

The problem with Microsoft’s account is essentially the reverse. You’re allowed to have the same email on both a personal and a business account. That’s great to keep things separated. Now you need to remember which account you used for that thing you’re trying to do. If you’re diligent about keeping work in the work account and personal content in the personal content, that’s not too bad. That’s not always possible and people aren’t always going to be good at it.

Here’s another scenario I’ve seen: an organization built a SharePoint portal system for partners across lots of organizations. Some of those partner organizations use Microsoft 365. Some do not. To let the partners in who are not using Microsoft 365, the portal has to allow logging in with consumer Microsoft accounts. In theory, the goal should be that every partner who does have Microsoft 365 business account uses their business account, while personal accounts are the fallback for everybody else.

In practice, it got a lot messier than that. Some people had business accounts, but didn’t understand they could use that, so they created a separate personal account, maybe using their same business email and maybe using a different email. Then they go to the portal and it says they don’t have access. They confirm they are logged in with their work email, so that’s strange. They give up and contact me for support. I dig into the Azure Active Directory and discover this partner used a personal Microsoft account, not a business account. So they end up needing to log out of their business email account they use for everything else and logging in with a personal Microsoft account with the same email address that they’re using for just this one thing probably by accident. That’s confusing.

This is the value of Google’s approach: **there is no confusing dialogue asking you whether you want to sign in with the business or personal account**. There is no need to remember what services are associated with which account on the same email.

## Winner?

For the majority of normal users in the majority of cases, Google’s scenario is better. It’s easier. Most average people just want a separate personal account from their business account using different emails and they’re not concerned about custom domains. Google’s approach enforces the simplicity that most people want.

For Microsoft power users like me who can keep it straight, their approach is better. I’ve considered whether I should move my primary personal email (a custom domain that was signed up in the days of Windows Live Domain) from a consumer Microsoft account to a Microsoft 365 account. If I do decide to do that, here’s how I would:

1. Put an outlook.com alias on the consumer account
2. Set up the domain and DNS on the Microsoft 365 business, create the user there
3. Delete the custom alias from the consumer account
4. Going forward, I can use the custom domain for the work account and the alias for the personal account

There is no scenario where I don’t lose massive amounts of personal data if I ever decided to put my custom domain in Google Workspace.

What Microsoft doesn’t offer is transferring an alias from one account to another. I have a better outlook.com alias on a different Microsoft account that I would love to switch to my main account, if I gave up the custom domain alias. I would pay extra for the ability to merge them / migrate the alias. As a few accounts with tech giants own more and more of our lives, I think it’s going to be more and more important that they offer a way to solve problems like this.

**The real ideal to me is probably to restrict it like Google, but have aliases like Microsoft.** That’s the best of both worlds to me: clear separation of accounts while having the flexibility if you need to make a change.
