---
title: "Vagrant: Oracle Linux VM"
date: 2021-11-10T21:34:00.000Z
author: Ryan Robinson
series: GitLab DevOps
tags:
    - Git, GitLab, and GitHub
    - DevOps
    - PHP
    - Linux
---

This is the first in [a series setting up a basic DevOps pipeline](/tags/gitlab-devops/) through local virtual machines, GitLab, a dev server, a staging server, and a production server. This first post will look at the local VM setup. The next step will move on to using this local VM in connection with GitLab for version control and CI/CD.

## Oracle Linux 8 LAMP Stack

The VM will be using [Vagrant](https://www.vagrantup.com/) and [Oracle VirtualBox](https://www.virtualbox.org/). Every user will need to install both, as well as the VirtualBox extensions, for this to work.

The configuration for the Vagrant box can be found on [my GitHub](https://github.com/ryan-l-robinson). For the sake of this demonstration, it is a fairly simple Oracle Linux 8, using a build in the Vagrant repository.

The first script, provision.sh, sets up the basic VM with a LAMP stack:

- Apache
- MySQL
- PHP 8.0
- Several PHP extensions
- [Composer](https://getcomposer.org/)

That’s a nice generic LAMP stack. If you want an Oracle Linux 8 LAMP stack for some other purpose, you can stop there.

## Drupal 9 Local Dev

In my case, the purpose of this vagrant box is for the first stage of development on a Drupal 9 website. So I’m going to go a few steps further to get it ready for the specific Drupal 9 project. That includes:

- Sync the user’s SSH key from their computer’s folder to the vagrant user’s home folder, so that [it can be used to connect to GitLab](/posts/2021/web-development-workflow/)
- Configures git including pulling the latest version of the code from the GitLab
- Imports a MySQL database if one is provided
- Self-signed keys so that https browsing will be possible

I may write up specific details of some of these later, but [the bottom line is the code in GitHub.](https://github.com/ryan-l-robinson/Oracle-Linux-LAMP) This part is in the provision script d9.sh, which also references a separate script specifically for the database import called importdb.sh.

## The User Experience

The idea is that at the end of this, the person setting up their machine has very few steps:

1. `Git clone` the repo for building the virtual machine on their PC
2. Put a copy of the database to import, if available, in the specified location
3. Put a copy of the SSH key into the specified location
4. Run `vagrant up` to build the virtual machine
5. SSH into the VM and then run the separate script for the Drupal 9 components
6. Optionally add the dev URL to the hosts file of their computer so that it is browsable at the URL and not just at localhost
