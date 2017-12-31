---
layout: default
title: FoldingText 2.1 Release
description: Highlights include new calc and stopwatch modes.
---

- Fixed Show All
- Added View > Open Link
- Added .calc mode
- Added .stopwatch mode
- Renamed .time mode to .schedule
- Added Focus Out menu item
- Added support for smart pairs: (), {}, etc.
- Added "soundName" option when displaying notifications
- Convert to list better handles level 0 body text
- Control-Return inserts line separator
- Added bottom padding and typewriter scrolling options to code. Not yet exposed in UI
- Hold down Command when clicking on tag to include tag descendants in results
- Space after snippet text will now expand the snippet. Use Option-Space to escape that behavior
- Added logging of path used to check licensing
- Added DefaultItalicSyntaxChar and DefaultBoldSyntaxChar options
- Don't show "Show License" menu item if not activated
- Documented clockTick as a public extension point
- Fixed Clicking on Node path links
- Fixed limit smart pairs to (),[], {}, and ""
- Copy License key into paddle version of the app
- Fixed parsing of web links containing underscores
- Fixed crash when node path contained empty string //""
- Try harder to open a file with an unknown plain text encoding
- Fixed crash when generating placeholder for special chars
- Fixed problem where .schedule would run after closing document
- Fold nodes command updated handle empty node at top of selection case better
- Added MultiMarkdown processor for Copy as (RTF, HTML, LaTex) conversions. End results is the Edit > Copy as options all work much better now
- Fixed escape key removes multiple cursors if used instead of doing autocomplete

Going forward Mutahhir Ali, who also worked with me to create FoldingText, will maintain FoldingText. Mutahhir will focus on bug fixes and keeping FoldingText working into the future.