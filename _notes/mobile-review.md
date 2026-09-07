# Mobile review — September 7, 2026

Implemented: repaired the collapsed navigation, added accessible naming and keyboard focus, enlarged phone menu and phone/tablet social touch targets to 44px, stacked the introduction through 800px, bounded portrait sizing including its border, and allowed long text to wrap. Display equations have independent horizontal scrolling.

## Verification

- Jekyll built successfully (existing Ruby/Sass deprecation warnings).
- Isolated headless Chrome with mobile viewport/touch emulation; no physical-device or Safari testing.
- Home, Publications, Blog, and 404: no horizontal page overflow at 320, 375, 390, 430, 600, 601, 768, 800, 820, 1024, and 1440 CSS pixels (44 page/width combinations).
- Menu starts closed and opens/closes by touch on every page through 600px.
- Keyboard Space opens menu; Publications link navigates successfully.
- Visually inspected screenshots of phone home (320 and 390px), publications (320px), blog and 404 (390px), open menu (390px), tablet home (768px), and desktop home (1440px).
- Screenshots saved in mobile-qa/. Blue menu highlights in touch screenshots are Chrome's tap feedback.
- The sole blog post is unpublished and was not included in the public-page checks; mathematical rendering was not exercised.
- Changes are local, not deployed. This underscore-prefixed notes folder is excluded by Jekyll.

## Deferred design ideas — none implemented

| ID | Idea | Reason | Status |
| --- | --- | --- | --- |
| D1 | Darken the pink publication-title color | Improve contrast for reading outdoors and on small screens | Deferred |
| D2 | Increase publication line spacing and the gap between entries | Make dense author lists easier to scan | Deferred |
| D3 | Add year/category jump links to Publications | Reduce scrolling through the long bibliography | Deferred |
| D4 | Consider a shorter introduction or earlier portrait placement on phones | Bring the personal introduction and selected research into view sooner | Portrait moved above introduction on phones at user request; shortening copy remains deferred |

## Follow-up

At user request, moved the portrait above the introduction at widths through 600px and set introductory text to compact single spacing (1.2 line height). Paragraph breaks remain.

Follow-up verification: repeated the 44 page/width checks and menu interaction checks successfully; visually reviewed the updated 390px home screenshot. Updated saved screenshots.

## Menu and portrait refinement

User authorized design refinements: phone portrait is now a centered 180px circular CSS crop; the original image is unchanged. Phone navigation uses a labeled native Menu button and an in-flow full-width panel, with a subtle active-page background. Added expanded-state semantics, Escape/outside-click dismissal and reset on breakpoint changes. Navigation remains available without JavaScript.

Verification: Jekyll build and 44 page/width checks passed; visually inspected final 320px home and 390px expanded menu. Escape, outside tap, keyboard activation, navigation and breakpoint reset passed in Chrome emulation. Desktop styling remains as before. Other deferred design ideas remain deferred.

## Full-width photo variant

At user request, replaced the circular phone portrait with a rectangular image spanning both screen edges directly beneath the header. Uses the original image proportions; introduction text keeps its side padding. Visually verified the 390px screenshot and repeated menu dismissal and breakpoint-reset checks. Updated 320px, 390px, and expanded-menu screenshots.
