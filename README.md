# Ryan Robinson Technology

This is the technology blog of Ryan Robinson, built using Eleventy. It is now mostly functional but has some room for improvement as noted below.

## TODO

- Fix pagination. Everything that should have a pager, including tag pages, does now, but it is inconsistent which buttons show up where.
- Update links in dev.to to point to the new addresses.
- Search functionality works but I don't think is very accessible. It auto-updates as text is entered, no button for a user to know that's going to happen, with no aria-live region. The Clear button also doesn't specify that it is a reset button. It also shows an excerpt from the body, not the specific tl;dr field I use elsewhere.
- Posts: add descriptions to all posts (must be in quotes to appear), review tags and series, remove categories that aren't doing anything, update image paths, update link paths.
- Posts by year page: these are possibilities implied by the URL patterns, but don't exist (tags and posts pages now exist). This should be solvable in the same way as pages for posts by tag.
