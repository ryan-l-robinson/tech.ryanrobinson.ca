---
title: "GitLab DevOps: GitLab Runner"
date: 2022-02-09T14:03:00.000Z
author: Ryan Robinson
tags:
  - Git, GitLab, and GitHub
  - DevOps
  - GitLab DevOps
---

This relates to [the GitLab DevOps process](/tags/gitlab-devops/). GitLab can automate deployment to a server, meaning that you do not have to log in to that server separately to pull the changes and carry out any other needed steps for the installation.

But there’s a catch: if your server is not publicly-accessible, such as behind a VPN, which is pretty common for development and staging servers, you’ll need to install a runner. This is because GitLab cannot initialize a connection to a hidden server, so the server will have to start the connection instead.

Here’s how I installed a GitLab runner on a server. This was specifically for Oracle Linux 8, but it will be pretty close for other distributions.

## Install GitLab Runner Package

First you need to install the GitLab Runner package on your server. GitLab will give you the instructions to do this, but here’s the general idea.

First, copy the package to your server, in the /usr/local/bin folder:

```bash
sudo curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64"
```

Add permissions to execute that file. You’ll need to be able to run it.

```bash
sudo chmod +x /usr/local/bin/gitlab-runner
```

Navigate to that folder and install:

```bash
cd /usr/local/bin
sudo ./gitlab-runner install --user=libgit --working-directory=/home/libgit
```

## Register a Runner

Once the runner is installed, you’ll need to register at least one runner on the server.

```bash
sudo ./gitlab-runner register --url https://gitlab.com/ --registration-token [token]
```

The script will prompt you with a few configuration options. Some are fairly obvious, but I’ll note a couple others.

The token can be found in the GitLab Project -&gt; Settings -&gt; CI/CD -&gt; Runners.

For the tag, I recommend having a standard convention across all your servers, like \[site shortcode\]-\[dev stage\]. For example, site1 will have:

- site1-dev
- site1-staging
- site1-prod1
- site1-prod2

And site2 will have:

- site2-dev
- site2-staging
- site2-prod

For what executor to use. Shell is the simplest, at least in a scenario like this of running it on a Linux server.

That’s it. It may take a bit to appear, but the runner will now be associated with the GitLab project.