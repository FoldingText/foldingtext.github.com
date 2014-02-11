---
layout: default
title: Create Scripts (FoldingText 1.3 Dev Only)
---
Use AppleScript to automate FoldingText and integrate with other AppleScript enabled apps.

## About

The AppleScript API exposes the "text contents" of your app and an "evaluate" command.

This is a different design then in FoldingText 1.1. There's no longer a fully separate AppleScript API for FoldingText. Instead for more complex needs you'll need to use the "evaluate" command to bridge from AppleScript to FoldingText's JavaScript plugin API:

- [Plugins](../plugins)

## Getting Started

Open "AppleScript Editor" and run:

```applescript
tell front document of application "FoldingText"
	text contents
end tell
```

That script is pretty simple. It uses the "text contents" property to get the text content of the front document. This is simple, but limited. If you want to do more complex things you will need to use the "evaluate" command and the JavaScript plugin API.

Open "AppleScript Editor" and run:

```applescript
tell front document of application "FoldingText"
	evaluate script "
	function(editor, options) {
		return editor.textContent();
	}"
end tell
```

That returns the same results as the first script, but uses the "evaluate" command instead of the "text contents" property. Evaluate takes a JavaScript function "script" parameter and evaluates it in the contents of the targeted FoldingText document.

This example shows how to pass in options:

```applescript
tell front document of application "FoldingText"
	evaluate script "
	function(editor, options) {
		editor.replaceSelection(
			options.start +
			editor.selectedText() +
			options.end
		);
	}" with options {start:"A", |end|:"B"}
end tell
```

To create more complex scripts:

1. Read through the plugin development document to learn about the JavaScript API.

2. Learn from existing [script extensions](/posts/extensions/scripts/) and use them as starting points for your own solutions.

3. Make sure to read the next section "Debugging Scripts" and use the debugger when writing your own scripts.

4. Ask questions in the [Extensions](http://support.foldingtext.com/discussions/extensions) support forum.

## Debugging Scripts

Use the "debug" suite command to debug your scripts. This example will get you started.

1. Open Help > [SDK Runner](../runner)

2. Click "Run Editor" in the SDK Runner window to open the editor

3. Click the "Inspector" toolbar item to open the SDK inspector

4. Run this script in "AppleScript Editor"

```applescript
set s to "
function(editor, option) {
	debugger;
	return editor.selectedText() + option;
}"

tell application "FoldingText"
	debug script s with options "!"
end tell
```

Notice that this script targets the application object instead of a document. And the command name is "debug" instead of "evaluate". The "debug" command is exactly like "evaluate" except it targets the SDK Runner instead of the current document. And that means you can step through your scripts execution line by line in the inspectors debugger.