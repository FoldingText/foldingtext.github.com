---
layout: default
title: FoldingText 1.2 Release
description: Highlights include Command mode and Javascript plugin API.
---

- Added Command Mode (Command-') interface.
- Added Plugin support (Help > Plugin API Documentation)
- Added Typewriter scrolling (View > Typewriter Scrolling)
- Run scripts from command mode (File > Open Scripts Folder)
- Changed to insert after fold when pressing return.
- Changed default node path from `/*///*` to `///*`.
- Changed AppleScript `read nodes` command to always return list.
- Select syntax when toggling bold, italic, and code.
- Reveal file links in Finder instead of trying to open them.
- Fixed some repetitive steps in the focus out process.
- Fixed syntax highlighting of mode declared on empty line.
- Fixed Reference links to now work with capital letters.
- Fixed crash when focusing out on heading with '/' character.
- Fixed error evaluating @index = 1 node path predicates.
- Fixed performance bug when many lines are folded.
- Fixed potential lost changes when editing after autosave.
- Fixed losing focus when zooming in and out.
- Fixed crash when ordered list is followed by unordered '*' list.
- Fixed crash when jumping to next spelling error when folded.
- Fixed crash when dragging and dropping text.
- Fixed crash when Moving Section Left when contained empty lines.
- Fixed crash when trying to open document that you don't have permission to read.

This release brings FoldingText one step closer to "Plain text productivity *platform* for geeks." FoldingText is now extensible through a Javascript plugin API. This release also include a *Command Mode* which provides a quick and consistent way to use and expose plugin commands without having to remember tons of keyboard shortcuts.