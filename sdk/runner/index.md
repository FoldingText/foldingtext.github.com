---
layout: default
title: SDK Runner (FoldingText 1.3 Dev Only)
---
Use the SDK (Help > SDK Runner) while developing and debugging your extensions for these benifits:

1. Open a web "inspector" for viewing the DOM and debugging your JavaScript plugins and LESS/CSS themes.

2. Uses uncompressed JavaScript sources so you figure out what's really going on.

3. Automatically reloads anytime you edit a file in FoldingText's application support folder for a faster development cycle.

4. Run FoldingText's test suite including any specs that you've included in your own plugins.

The SDK's "Run Editor" link opens FoldingText's editor with all plugins and themes loaded. It's the same as the editor used in a normal FoldingText document, except the sources are not compressed and it has no connection to native Cocoa API's. That second part means a few things wont work the same including:

- Spell check won't work.

- Text you enter won't be saved when FoldingText quits.

- FoldingText's popup menu won't be used, instead you'll just get default web browser popup menu.

- Many native menu items and keyboard shortcuts won't be used. But any keyboard bindings that are set in FoldingText's javascript code (such as in a plugin) will be used.