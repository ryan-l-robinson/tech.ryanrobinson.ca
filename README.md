# Ryan Robinson Technology

This is the technology blog of Ryan Robinson, built using Eleventy. It is now mostly functional but has some room for improvement as noted below.

## TODO

- Search functionality works but I don't think is very accessible. It auto-updates as text is entered, no button for a user to know that's going to happen, with no aria-live region. The Clear button also doesn't specify that it is a reset button.
- Posts: add descriptions to all posts, review tags and series, remove categories that aren't doing anything, update image paths, update link paths.
- Posts by tag page: solve pagination. Delete the postslist-no-pagination template. [Possible lead on doing that](https://desmondrivet.com/2022/03/23/eleventy-pagination) but it doesn't explain in enough detail for me to put together what I need to do yet, or possibly that doesn't work with eleventy 3.0.
- Posts by year page: these are possibilities implied by the URL patterns, but don't exist (tags and posts pages now exist). This should be solvable in the same way as pages for posts by tag.
- Start to phase out ryanrobinson.technology and point everything here instead, including updating links in dev.to.
- Add broken link checker? 3.0 does protect against references to non-existent images, but not against links.
