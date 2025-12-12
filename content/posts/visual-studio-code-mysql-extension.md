---
title: "Visual Studio Code: MySQL Extension"
date: 2021-09-29T20:16:00.000Z
author: Ryan Robinson
tags:
    - Visual Studio Code
---

In a recent post I talked about [how great MySQL Workbench is](/posts/2021/mysql-workbench/). A few days later, I discovered a Visual Studio Code extension simply called MySQL. It is not as robust in some ways as Workbench (e.g. bulk importing and exporting).

However, it does meet all the same core functionality such as:

- browse the tables and databases available to the user, with all the data kept in nice clear and easy-to-read lines
- write your own queries a table and get results in a nice table where you can do things like click sort by different headers
- edit entries directly in the table (no form editor mode like Workbench)

It does not do all the more intensive functionality that Workbench does, like importing and exporting with CSV files, but that’s the kind of thing that you likely don’t have to do regularly.

The big advantage that it has over Workbench is being a part of Visual Studio Code which you are already using. That means a few little things can go faster and more lightweight on your computer’s resources. The two little things that I can imagine make a meaningful difference over time are:

- less steps to login, since you are already SSHed to the host
- the ability to have a database open as a tab right beside the code, rather than jumping back and forth between Code in one window and Workbench in another

Of course it’s great to also have Workbench available in your toolbox if you need more intensive functionality, but speaking for myself, I think I might start relying on this extension more often for the majority of my lightweight work.
