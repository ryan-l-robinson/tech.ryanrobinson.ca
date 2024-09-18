---
title: "Local Development: Vagrant, Docker Desktop, and GitPod"
date: 2022-07-03T01:43:09.000Z
author: Ryan Robinson
tags:
  - DevOps
---

I've now broken down some of what I've learned about different virtualization/containerization options:

- Vagrant
- Docker
- GitPod

I also learned that each of these options have some pro's and con's attached to consider.

## Vagrant Pro's

Vagrant is likely to be the most intuitive to develop since it maps the closest to building a server (other than building a server), if you are used to building servers. It's not a whole new syntax and way of thinking to learn.

## Vagrant Con's

It is very slow to build the VM, when it works at all. Fully building the VM and starting the Drupal 9 website I was using this for could take 6-8 hours.

It continues to be very slow in usage afterward: browsing the site, running composer updates from the terminal, etc.

It can be very buggy, with issues like arbitrarily changing permissions on a file so that you can no longer delete them from your computer even with admin access. I've got files on my computer that as far as I can tell there is no way to delete. Eventually it stopped working for me entirely.

There are no extra tools for integration with [Visual Studio Code](/tags/visual-studio-code/), although you can ssh to a vagrant VM the same way you can to any other server.

## Docker Desktop Pro's

Compared to Vagrant, Docker is much faster to build the image, at about 1-2 hours for the same Drupal 9 site.

It has great integration with Visual Studio Code via the .devcontainer functionality, including installation of any extensions and pre-defining settings for all users. I have detailed much of my setup here in [my Drupal Docker series](/tags/drupal-docker/).

It has a strong community to draw on for support and for images. It's the most popular option now and that means you'll be able to find what you need more easily than for other options.

## Docker Desktop Con's

It is less intuitive to develop the images if you haven't used Docker before. Learning the new syntaxes (Dockerfile, Docker compose) is straightforward, but there is a shift in thinking to containers.

It is for the most part about as slow as vagrant to operate after the site is built, although you can improve that a bit by editing your wslconfig file to allot more RAM.

## GitPod Pro's

GitPod is much faster, both to build and to use afterward, although the build time does sometimes vary wildly and stall out. Even the odd slow time is at least as fast as Docker, but the inconsistency can be disruptive. It's possible that this is not a serious problem and it only happens in my stress test scenarios like constantly building new containers.

The pre-build functionality allows for the build to happen before you actually need it.

It does facilitate keeping everybody on the same code base since it builds directly from the GitLab repository each time. There's little concern about getting a commit behind.

The Git integration is the strongest. Since it is already tied to your GitLab/GitHub/Bitbucket account, the push and pull does not require extra configuration like syncing an SSH key file and entering a passphrase each time.

It allows for defining what to do when each port opens, e.g. open in the browser when Apache starts up so you can browse the site. This is a small time saver.

It does not tie up your computer's resources and does not require at least 32GB of RAM like the locally-hosted options do.

It does not need your computer to support virtual machines / containers (i.e. Windows 10/11 Pro).

## GitPod Con's

It's great when you're using a Debian-based image, but does not support other Linux variants or multiple containers.

Data for their SaaS offering is held in the US and EU. Those are the only data residency options. Canada where I am is not an option. If you're concerned about data staying in your country and you're not in the US or EU, this is a problem. Otherwise, they do offer self-hosting on Azure/AWS/Google Cloud where you can control where it gets stored, but if you're going that far, is it that much better than other options?

It does not automatically open VS Code Desktop. It will open in the browser that looks exactly like VS Code, then you can prompt it to open VS Code desktop, which will take clearing a couple other permission prompts. This isn't a big deal if you're opening VS Code once per day, but if you're in and out for a lot of quick changes, that does add up.

In my context, it also cannot open VS Code at all if I was on my work network, leaving me to only edit in the browser. It did work early on, and maybe it will work again. I also have no idea to what degree this is on GitPod's end vs our end, so I don't say this as much to negatively review GitPod as to say it is another factor to consider.

It does not support all Visual Studio Code extensions - only those which are in the Open VSX directory - which means a few absences like [GitHub Copilot](/websites/github-copilot/).

## Local Server Pro's

You could sidestep all of these questions about local development options and build your own servers.

This makes it easiest to guarantee a match with your production servers.

There shouldn't be any performance issues or use up any of your computer's resources.

## Local Server Con's

This approach doesn't scale very well if you work on a team or on multiple projects. You would need at least one virtual server per person per project. All of that scale is being paid for while not being used most of the time.

Depending on your organization structures, you may need to work across different departments to maintain the servers.

There are no extra tools for integration with Visual Studio Code, although you can ssh to it the same way you can to any other server.

## Summary

In my day job context, we settled on using Docker Desktop. It's not as fast as GitPod, but it's close enough and it was worth the trade to be able to build containers exactly as we want and with no data residency concerns because nothing leaves our devices.

In my experimental and demo stuff that often gets profiled here, I'm often using GitPod. It's fast, I can run it from any device, and it's great for demonstration purposes since I can set it up so that anybody can have a site running my code within a couple minutes.
