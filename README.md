## Overview

This is the technology blog of Ryan Robinson, built using Eleventy.

## TODO

It is now mostly functional but has some room for improvement as noted below.

- Remove "posts" from the URL path for each post. That's not needed. Everything but the homepage and tag pages are posts.
- Tag pages: add support for tag descriptions that show up on the tag's page. A first draft is in the theology site. It can be cleaned up to have less repetition combined with how the home page does an introduction, and then I'll need to add descriptions for all the tags.
- Design a 404 page instead of just text.
- Restore pagination on the homepage, without breaking it on tag pages.
- There is functionality for exluding drafts. Should I be putting those in posts right away and commit to the repository, instead of moving the file from a different folder when it's ready? The downside is that there is more public record of older drafts (in the repo, not on the site) which could expose something that isn't quite ready yet. Or I could do some combination of both, where it gets into the posts folder once I know it has nothing sensitive in it, even if there is more to finish otherwise.
- Posts: Review tags on existing posts for consistency
- Posts: add descriptions to all posts (must be in quotes to appear), review tags and series, remove categories that aren't doing anything, update image paths, update link paths.
- Posts by year page: these are possibilities implied by the URL patterns, but don't exist (tags and posts pages now exist). This should be solvable in the same way as pages for posts by tag.
- Update links in dev.to to point to the new addresses.
- Search functionality works but I don't think is very accessible. It auto-updates as text is entered, no button for a user to know that's going to happen, with no aria-live region. The Clear button also doesn't specify that it is a reset button. It also shows an excerpt from the body, not the specific tl;dr field I use elsewhere.
