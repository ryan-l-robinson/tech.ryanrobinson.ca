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

I have been considering how best to arrange my personal projects in terms of which git repository to use. There are at least four interesting providers to consider.

## GitLab

I use [GitLab](https://about.gitlab.com) the most at work, in both my previous job and my current one. I've worked in both the cloud gitlab.com solution and the on-premise one. They are the same thing except for a few logical differences, like that instead of paying by the GitLab CI/CD minutes used, you pay to maintain the runners on servers yourself.

There's a lot I like about it, including having robust project management tools built in. It's got a lot of features, but it's all well organized and easy to use in my opinion. Maybe that's simply coming from me being the most used to it, but I find it the most pleasant and intuitive while also being the most powerful.

It is easy to find your way around things like:

- Issues
- Milestones
- Branches
- Tags
- Releases
- Merge Requests (aka Pull Requests)
- Container Repository
- CI/CD Pipeline runs
- Pages for hosting static sites
- Project settings, as distinct from user settings

It's also the only one of these that allows you to group your projects, which is really nice once you have more than a few projects for organization as well as for managing cascading permissions if you're working as part of a team.

In raw feature set and usability, I see GitLab as the clear winner.

## GitHub

I have used GitHub the most for my personal projects. It's what hosts this website, at least as of now (see the conclusion below).

It's more streamlined than GitLab is, not throwing as many project management tools at you, but still offering the most important things: the repository itself, issue tracking, CI/CD, and free website hosting for static sites.

It can't be hosted on-premise. You can only use their servers.

One minor advantage it has over the others is that it is the only one of these that makes it easy to edit code and view issues with an official mobile app. GitLab does have a few third-party apps out there, and I have found a new general git sync app which is well reviewed and I will try out, but GitHub is the only one that has a fully featured official app.

Of course, the real biggest argument for it is the network effect: that's where a lot of projects are, especially free open source ones. If you're trying to find a project to contribute to, or download a release of, or report an issue to, there's a high chance it will be on GitHub.

## Worktree and Codeberg

I started writing these two up separately, then immediately upon creating accounts realized they are both based on the exact same software. There are not a lot of differences between them obviously visible in functionality; there might be a few, but not enough to worry about.

[worktree.ca](https://about.worktree.ca/) is a new entry from a Canadian for-profit company. [codeberg.org](https://codeberg.org) is a more established non-profit based in the European Union. Worktree gets some points for being Canadian. Codeberg gets some points for being non-profit and in the EU I would consider almost as good as Canadian, since at least they have some privacy laws.
### Features

There are definitely some good things to note:

- The user interface is bland, but simple and easy to understand.
- There is support for 2-factor authentication as well as passkeys.
- They both have some degree of issue tracking, pull requests, and milestones, all easily accessible from the top menu as well as on a per-project basis.
- The issue tracking supports labels, milestone assignment, project (maybe that's what GitLab calls epics), assignee, time tracking, due date, dependencies. On issue tracking it is pretty close to GitLab, maybe even a little bit better than GitHub.

For the simple repositories where I am just sharing code samples, either one would be entirely adequate.

### Pricing and Static Site Hosting

What really sets Codeberg and Worktree apart is static site hosting.

With Worktree, the free tier is the most limited of the four options, by a wide margin. You get zero action minutes for free. GitLab, GitHub, and Codeberg give enough to handle some simple personal uses like hosting this website. Worktree does not. It can still handle hosting all the simple code repositories like when I simply want to share a Drupal module I developed, but not if I want to run any kind of CI/CD on it, including building static sites in 11ty.

If I do want some Actions minutes, I will need to come up to the Worktree Developer tier, which is $9/month. They do also have some language on the pricing page where they encourage paying solely for the sake of supporting a Canadian business, which I'm not really complaining about because maple-washing is definitely a thing right now for very good reasons. I might like it more if it was non-profit like Codeberg, but it's reasonable if you are going to make good use of it, especially professionally.

That also doesn't include static site hosting. For that, I'd need to also be signed up for the associated Worktree Cloud service. That is $3/month per static site, but a little more per bandwidth used - not much for my sites that are mostly for my own notes. I am going to grumble more about this. Static sites with virtually no traffic, like mine where I acknowledge fully that I am mostly writing this as my own documentation store, can add up to costs a lot. A WordPress hosted site on a shared server is a similar price, while offering a lot more functionality.

In total, if I want to have my current three static sites, with Actions minutes to build two of them, that's $18-$20/month. That may be fair for a business or even for a freelance professional wanting flexibility. For me just wanting to share some code and some notes, it's a little steep and I'm inclined to think there are better uses for that money in my quest to rely less on US Big Tech. When I shared this on Mastodon, somebody replied pointing out that I could buy my own VPS, host my own forgejo with CI/CD and multiple static sites at that price.

Part of why I say that is by the comparison with Codeberg. They have no suggested pricing, but survive off donations. As far as I can see so far, there is no limit on Actions minutes or on static sites hosted. There is a limit on storage, at 750MB for the repositories, which should be plenty for a while with my relatively few pictures and the rest being text files.

## Conclusion

At least for now, I'm going to try splitting with:

- GitLab keeps the projects that are demonstrating CI/CD functionality which I have mostly learned from my work use of GitLab. There's not much choice there.
- Codeberg gets everything else, including the static sites as well as all the demo repositories like shared Drupal modules.
- GitHub I'll keep the account to interact with other repositories, but stop having that as my primary home for anything.
- Worktree I'll keep the account and maybe check in occasionally to see if anything has changed enough to change my mind, but otherwise I'm just lending one more user account to their metrics to try to argue that Canadians do want a homegrown option. I do want a homegrown option. I just can't justify those costs for my minimal needs.
