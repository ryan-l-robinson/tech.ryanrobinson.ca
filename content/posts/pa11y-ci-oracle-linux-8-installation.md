---
title: "pa11y CI: Oracle Linux 8 Installation"
date: 2021-12-13T19:17:00.000Z
author: Ryan Robinson
tags:
  - Accessibility
  - Linux
---

I’ve mentioned before that [pa11y](https://github.com/pa11y/pa11y-ci) is a great tool for accessibility testing, specifically in the context of [the koa11y desktop tool.](/posts/2021/koa11y-accessibility-testing-tool/) In this post, I’ll run through installing pa11y-ci on an Oracle Linux 8 server.

## NodeJS

Pa11y requires [NodeJS](https://nodejs.org/en/). To add that to Oracle Linux 8, run this command:

```bash
sudo yum install nodejs -y
```

If you want to add an efficiency check within a larger bash script, you can first check whether it is already installed:

```bash
#Install NodeJS and NPM
if [[ "$(yum list --installed | grep nodejs | wc -l)" -eq 0 ]]; then
    sudo yum install nodejs -y
fi
```

## Pa11y

Now you can install Pa11y itself using npm:

```bash
sudo npm install pa11y -g --unsafe-perm=true --allow-root
```

Note the extra flags for unsafe-perms and allow-root, which don’t sound like good things. But without them, it will fail to have the permissions to write everything in all the directories it needs to be able to write.

As above, you can add an efficiency check if you’re doing this within a script:

```bash
#Install pa11y
if [[ "$(npm list nodejs | wc -l)" -eq 0 ]]; then
    sudo npm install pa11y -g --unsafe-perm=true --allow-root
fi
```

## Dependencies

Pa11y is now installed, but trying it out, it won’t actually work, citing other libraries that are missing. There are several of these. To install all of them:

```bash
#Install libraries
sudo yum install -y pango.x86_64 libXcomposite.x86_64
 libXdamage.x86_64 libXext.x86_64 libXi.x86_64 libXtst.x86_64 cups-libs.x86_64 libXScrnSaver.x86_64 libXrandr.x86_64 GConf2.x86_64 alsa-lib.x86_64 atk.x86_64 gtk3.x86_64 nss libdrm libgbm

#Install fonts
sudo yum install -y xorg-x11-fonts-100dpi xorg-x11-fonts-75dpi xorg-x11-utils xorg-x11-fonts-cyrillic xorg-x11-fonts-Type1 xorg-x11-fonts-misc
```

As above, in a scripting context you might want to check if it is already installed, with something like this:

```bash
if [[ "$(yum list --installed | grep pango.x86_64 | wc -l)" -eq 0 ]]; then
    sudo yum install pango.x86_64 -y
fi
```

[The full script is available in my GitHub.](https://github.com/ryan-l-robinson/Drupal-Oracle-Linux-scripts/blob/main/pa11y.sh)
