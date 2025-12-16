## Overview

This is the technology blog of Ryan Robinson, built using Eleventy.

## TODO

It is now mostly functional but has some room for improvement as noted below.

My workflow:

- FrontMatter dashboard: sort out how to add media so they are more easily browseable. Get rid of Categories showing as a taxonomy, but add Series.
- FrontMatter sidebar: can Series select from previous terms used, with the ability to add a new one? What about Content Preview images?
- There is functionality for excluding drafts. Should I be putting those in posts right away and commit to the repository, instead of moving the file from a different folder when it's ready? The downside is that there is more public record of older drafts (in the repo, not on the site) which could expose something that isn't quite ready yet. Or I could do some combination of both, where it gets into the posts folder once I know it has nothing sensitive in it, even if there is more to finish otherwise.
- Set up snippets as some general templates that can be used with Front Matter in VS Code, rather than the templates file that I often didn't use anyway.

Existing Content:

- Posts: Review tags on existing posts for consistency
- Posts: add descriptions to all posts (must be in quotes to appear), review tags and series, remove categories that aren't doing anything, update image paths, update link paths.
- Update links in dev.to to point to the new addresses.

Theme Room for Improvement:

- Search functionality with Pagefind worked but was not very accessible. I've started a transition to elasticlunr, which seems to be much better and offer more control to fix issues. But this still has lots to do: add the excerpt, make sure the full content is indexed at a lower weight than other metadata, and a lot of styling for how it should show up on the site.
- I now have tag descriptions that show up on those tag pages. This should also show up as hover title text over links to the tag in various places they appear.
- This won't be a problem for a while but the paths for years and the paths for pages of all posts could conflict, once there is 2021 pages of posts. It probably won't ever be an issue for me because I'm not writing that much, but in principle it should still have some way to handle it.
