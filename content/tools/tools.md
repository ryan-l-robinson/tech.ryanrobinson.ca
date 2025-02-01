---
layout: layouts/sidebar.njk
title: Favourite Technical Tools
eleventyNavigation:
  key: Favourite Technical Tools
  order: 3
---

Here are some of my favourite tools I make use of in my tech work. Some of these do have more posts about their usage.

## Text Editor

[Visual Studio Code](https://code.visualstudio.com/): an editor that manages to be extremely powerful with a lot of features and extension support even while being relatively lightweight. [See blog posts tagged as Visual Studio Code](/tags/visual-studio-code/).

## Terminals

[Windows Terminal](https://apps.microsoft.com/store/detail/9N0DX20HK701?hl=en-us&gl=US): a pleasant terminal in Windows that incorporates all your shells in one app. This is now default on Windows 11 but requires downloading it separately on Windows 10.

[PuTTy](https://putty.org/): a great option to save multiple SSH connections to servers, with a lot of configuration settings to navigate different requirements. If you're connecting to the same set of websites regularly, this can save you some time and some typos.

## Code Repositories

Git with [GitLab](https://about.gitlab.com/) or [GitHub](https://github.com/): for the most part I use GitLab professionally and GitHub personally. GitLab gives a bit more project management tools out of the box, but they're both great. [See posts tagged as Git, GitLab, and GitHub](/tags/git-gitlab-and-github/).

## Accessibility Testing

[WAVE](https://wave.webaim.org/): testing tools including a browser extension that can help highlight accessibility issues while you browse the site.

[Pa11y](https://pa11y.org/): testing tools that are especially great from a command line, including being able to incorporate them into CI/CD pipeline tests.

[Playwright with Axe](https://playwright.dev/docs/accessibility-testing): Playwright is a general testing tool that can simulate browser activity. Axe-core provides accessibility test results. Putting them together, you can carry out actions - as simple as loading a page or more complicated such as expanding menus - and then run accessibility tests. These can be run well within CI/CD automated tests as well as locally with great VS Code support.

[See posts tagged as accessibility](/tags/accessibility/).

## Database Management

[MySQL Workbench](https://www.mysql.com/products/workbench/): a heavy-duty database tool that lets you see the database in convenient table views, edit inline, and run other commands. [See my blog post about MySQL Workbench](/posts/2021/mysql-workbench/).

[MySQL Extension in Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=cweijan.vscode-mysql-client2): extension to access MySQL servers, including viewing in an easy-to-read table format and running queries.
