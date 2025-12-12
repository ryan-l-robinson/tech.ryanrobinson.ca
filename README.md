## Overview

This is the technology blog of Ryan Robinson, built using Eleventy.

## TODO

It is now mostly functional but has some room for improvement as noted below.

- Finish solving and testing pagination. I think the majority of it works now, but maybe not all situations.
- Tag pages: add support for tag descriptions that show up on the tag's page. A first draft is in the theology site. It can be cleaned up to have less repetition combined with how the home page does an introduction, and then I'll need to add descriptions for all the tags.
- Add similar to the tag pages, but by year, so that every year has a page of all posts that year, with pagination.
- Add a sidebar block similar to the tags-list sidebar block, showing links to each posts by year page.
- Design a 404 page instead of just text.
- FrontMatter: sort out how to add media and data schema so they are more easily browseable. Get rid of Categories showing as a taxonomy, but add Series.
- There is functionality for excluding drafts. Should I be putting those in posts right away and commit to the repository, instead of moving the file from a different folder when it's ready? The downside is that there is more public record of older drafts (in the repo, not on the site) which could expose something that isn't quite ready yet. Or I could do some combination of both, where it gets into the posts folder once I know it has nothing sensitive in it, even if there is more to finish otherwise.
- Set up snippets as some general templates that can be used with Front Matter in VS Code, rather than the templates file that I often didn't use anyway.
- Posts: Review tags on existing posts for consistency
- Posts: add descriptions to all posts (must be in quotes to appear), review tags and series, remove categories that aren't doing anything, update image paths, update link paths.
- Update links in dev.to to point to the new addresses.
- Search functionality works but I don't think is very accessible. It auto-updates as text is entered, no button for a user to know that's going to happen, with no aria-live region. The Clear button also doesn't specify that it is a reset button. It also shows an excerpt from the body, not the specific tl;dr field I use elsewhere.
