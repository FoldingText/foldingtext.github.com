---
layout: default
title: Create Scripts (FoldingText 1.3 Dev Only)
---
Use AppleScript to automate FoldingText and integrate with other AppleScript enabled apps. You should first understand:

- [Plugins](../plugins)

## About

The AppleScript API simply bridges to the plugin API.

This is a different design then in FoldingText 1.1. There's no longer a separate AppleScript API for FoldingText. Instead there's just the "evaluate" command witch bridges between AppleScript and FoldingText's JavaScript environment.

## Getting Started

Open "AppleScript Editor" and run:

```applescript
set s to "
function(editor, option) {
	return editor.selectedText() + option;
}"

tell front document of application "FoldingText"
	evaluate script s with options "!"
end tell
```

The script result is the selected text with "!" appended.

The "evaluate" command is a bridge from AppleScript to FoldingText's internal [JavaScript plugin API](../plugins). You pass in a JavaScript function and optional parameters over the bridge. That function is evaluated within FoldingText. The results are sent back to AppleScript.

To create more complex scripts:

1. Read through the plugin development document to learn about the JavaScript API.

2. Learn from existing [script extensions](/posts/extensions/scripts/) and use them as starting points for your own solutions.

3. Ask questions in the [Extensions](http://support.foldingtext.com/discussions/extensions) support forum.