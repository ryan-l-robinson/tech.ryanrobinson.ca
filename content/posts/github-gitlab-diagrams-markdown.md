---
title: "GitHub and GitLab: Diagrams in Markdown"
date: 2022-03-10T01:00:00.000Z
author: Ryan Robinson
description: "Using Mermaid for creating diagrams in markdown."
tags:
  - Git, GitLab, and GitHub
---

GitHub recently announced [a great new feature that allows for generating diagrams within markdown](https://github.blog/2022-02-14-include-diagrams-markdown-files-mermaid/). I tested this out and it also works in GitLab.

If you’re writing a lot of documentation in markdown, as you probably are if you’re using GitHub or GitLab, this can be really nice when you need diagrams, like mapping out your CI/CD system or server maps. You can build diagrams much faster than putting something together in Photoshop or some other diagramming tool that exports an image which you then need to upload.

This is all it takes, building off the standard code markdown “` markers and using arrows to indicate links between objects.

```markdown
mermaid
  graph TD;
      A-->B;
      A-->C;
      B-->D;
      C-->D;
```
