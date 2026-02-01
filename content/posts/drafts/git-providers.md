---
title: Git Hosting Providers
date: 2026-02-10T14:46:00.000Z
author: Ryan Robinson
description: "Comparing some pro's and con's about GitLab, GitHub, Worktree, and Codeberg"
series: Big Tech Alternatives
tags:
  - "Tech and Society"
  - "DevOps"
---

I'm considering how best to arrange my personal projects in terms of repository.

## GitLab

I use GitLab the most at work, in both my previous job and my current one. I've worked in both the cloud gitlab.com solution and the on-premise one. They are the same thing with a few differences, like that instead of paying by the GitLab CI/CD minutes used, you pay to maintain the runners on servers yourself.

There's a lot I like about it, including so many other project management tools built in. It is by far the most robust of those that I've actually tried. It's got a lot of features, but it's all well organized and easy to use in my opinion. Maybe that's simply coming from me being the most used to it, but I find it the most pleasant and intuitive while also being the most powerful.

It is easy to find:

- Issues
- Milestones
- Branches
- Tags
- Releases
- Container Repository
- CI/CD Pipeline runs
- Project settings, as distinct from user settings

It's also the only one of these that allows you to group your projects, which is really nice once you have more than a few projects.

## GitHub

I have used GitHub the most for my personal projects. It's what hosts this website, at least of now (see the conclusion below).

It's a bit more streamlined than GitLab is, not throwing as many project management tools at you, but still offering the most important things: the repository itself, issue tracking, CI/CD, and free website hosting for static sites.

It also can't be hosted on-premise. You can only use their servers.

It is the only one of these that makes it easy to edit code and view issues with an official mobile app.

## Worktree and Codeberg

I started writing these two up separately, then immediately upon creating accounts realized they are both based on the exact same software. There are not a lot of differences between them obviously visible in functionality. [worktree.ca](https://about.worktree.ca/) is a new entry from a Canadian for-profit company. [codeberg.org](https://codeberg.org) is a more established non-profit based in the European Union. Worktree gets some points for being Canadian. Codeberg gets some points for being non-profit and in the EU I would consider almost as good as Canadian, since at least they have some privacy laws and it isn't funding the US tech broligarchy.

### Worktree/Codeberg Positives

There are definitely some good things to note:

The user interface is bland, but simple and easy to understand.

There is support for 2-factor authentication as well as passkeys.

They both have basic options for issue tracking, pull requests, and milestones. That's not nearly as robust of task management as GitLab, or even GitHub, but it's probably everything I need for anything I'm working on outside of my main professional job.

For the simple repositories where I am just sharing code samples, either one would be entirely adequate.

### Pricing and Static Site Hosting

With Worktree, the free tier is a little limited compared to GitLab. You get zero action minutes for free. GitLab and GitHub give enough to handle some simple personal uses like hosting this website. Worktree does not. It can still handle hosting all the simple code repositories like when I simply want to share a Drupal module I developed.

If I do want some Actions minutes, I will need to come up to the Worktree Developer tier, which is $9/month. They do also have some language on the pricing page where they encourage paying solely for the sake of supporting a Canadian business, which I'm not really complaining about. I might like it more if it was non-profit like Codeberg, but it's reasonable if you are going to make good use of it, especially professionally.

That also doesn't include static site hosting. For that, I'd need to also be signed up for the associated Worktree Cloud service. That is $3/month per static site, but a little more per bandwidth used - not much for my sites that are mostly for my own notes. So in total, if I want to have my current three static sites, with Actions minutes to build two of them, that's $18-$20/month. That may be fair for a business or even for a freelance professional wanting flexibility. For me just wanting to share some code and some notes, it's a little steep and I'm inclined to think there are better uses for that money in my quest to rely less on US Big Tech. When I shared this on Mastodon, somebody replied pointing out that I could buy my own VPS, host my own forgejo with CI/CD and multiple static sites at that price.

Part of why I say that is by the comparison with Codeberg. They have no suggested pricing, just survives off donations. As far as I can see so far, there is no limit on Actions minutes or on static sites hosted. There is a limit on storage, at 750MB for the repositories, which should be plenty for a while with my relatively few pictures.

### Worktree vs Codeberg Differences

The only differences between the two in terms of functionality, as far as I have found so far:

- Worktree has an associated cloud platform, which I haven't done any testing with
- Codeberg has a slightly nicer colour palette in my opinion, with its softer blue instead of the forest green
- Codeberg includes pronouns for the profile, which Worktree does not
- Static site hosting? Not clear if either have that included or not. I think Codeberg does have some for free, while Worktree does not.

## Conclusion

At least for now, I'm going to try splitting with:

- GitLab gets the static sites where they can be hosted for free, as well as the projects that are demonstrating CI/CD functionality
- Worktree gets the majority of the smaller repositories that are straight code to share, no Actions needed.
- GitHub I'll keep the account to interact with other repositories, but stop having that as my primary home for anything
- Codeberg?
