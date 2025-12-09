## Overview

This is the technology blog of Ryan Robinson, built using Eleventy. It is now mostly functional but has some room for improvement as noted below.

## TODO

- Header: larger font for site title?
- Styles on sidebar: it doesn't need that much space for the headings.
- Styles on sidebar: I don't really like the dashed border. 
- Posts: Review tags on existing posts for consistency
- Posts: add descriptions to all posts (must be in quotes to appear), review tags and series, remove categories that aren't doing anything, update image paths, update link paths.
- Can I block directly viewing the "pages" that are really just there to be able to put in the sidebar blocks? About and Posts By Tag? If I can't block Posts by Tag as a page, I need to at least stop showing the sidebar alongside the main page which is redundant.
- If I can block directly seeing the Posts by Tag page, I shouldn't need the show_sidebar metadata on pages, which is currently not working anyway.
- Posts by year page: these are possibilities implied by the URL patterns, but don't exist (tags and posts pages now exist). This should be solvable in the same way as pages for posts by tag.
- Update links in dev.to to point to the new addresses.
- Search functionality works but I don't think is very accessible. It auto-updates as text is entered, no button for a user to know that's going to happen, with no aria-live region. The Clear button also doesn't specify that it is a reset button. It also shows an excerpt from the body, not the specific tl;dr field I use elsewhere.
