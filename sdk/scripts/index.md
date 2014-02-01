---
layout: default
title: Create Scripts (FoldingText 1.3 Dev Only)
---
Use AppleScript to automate FoldingText and integrate with other AppleScript enabled apps. You should first understand:

- [Nodes](../nodes)
- [Node Paths](../nodepaths)

## Getting Started

Open "AppleScript Editor" and run:

```applescript
tell front document of app "FoldingText"
	read text
end tell
```

The text of the open FoldingText document should display in the result panel. You are scripting!

To create more complex scripts:

1. Use the rest of this document as a reference.

2. Use these [syntax example scripts](/posts/extensions/scripts/syntax-examples-script.html) to see examples of all the different scripting commands.

3. Learn from the existing [script extensions](/posts/extensions/scripts/) and use them as starting points for your own solutions.

4. Ask questions in the [Extensions](http://support.foldingtext.com/discussions/extensions) support forum.

## Referencing Content

When scripting you'll often need to refer to content in your document. FoldingText's scripting API provides a number of ways to do this. You'll use these "at" parameters in many places.

ids
:	List of node ids.

range
:	A range of text, defined by a "location" and "length". Use "-1" for either value to refer to the last position in your document.

indexes
:	List of node line indexes (line numbers). Use -1 to index the node representing the last line in your document.

path
:	A node path that is evaluated to determine which nodes are referenced.

branch
:	Often commands will take a "with branch" modifier. When "with branch" is included the command will target the entire branch of each node. For example if you delete a node by id "with branch", then that node is deleted AND any descendant nodes.

## Scripting Text Content

Use "read text" and "update text" to work with your document in plain text form. This script returns the text of all "@today" nodes:

```applescript
tell front document of app "FoldingText"
	read text at path "//@today"
end tell
```

## Scripting Node Content

Use "create nodes", "read nodes", "update nodes", and "delete nodes" to work with your document as a node tree. Nodes are exposed to the scripting API as records with the following keys:

id
:	Unique identifier that never changes as long as the document stays open.

type
:	The node's "type" including "heading", "unordered", etc.

level
:	Indentation level, starting at 0.

mode
:	Declared mode, `timer` or `todo` for example.

tags
:	Record of tag names and there values.

tagNames
:	List of tag names, hack needed by AppleScript since it can't iterate over the key names in the tags record.

text
:	Text content minus type syntax characters and trailing tags.

line
:	Entire text content of the node.

textIndex
:	Node's starting character index in the document text.

lineIndex
:	Node's line index in the ordered list of all nodes in document.

childIndex
:	Node's child index in its parent's list of children.

parentID
:	id of the node's parent.

firstChildID
:	id of the node's first child.

lastChildID
:	id of the node's last child.

previousSiblingID
:	id of the node's previous sibling.

nextSiblingID
:	id of the node's next sibling.

When you create nodes you don't need to provide values for each possible key in the node record. In fact many of the keys will be ignored even if you do provide them. These are the keys considered when creating a new node. You don't need to include them all.

type
:	Type. heading, unordered, body, etc.

level
:	Indentation level, starting at 0.

mode
:	Declared mode; "timer" or "todo" for example.

tags
:	Record of tag names and there values.

text
:	Text content minus type syntax characters and trailing tags.

line
:	Entire text content of the node.

By default when you create a new node it will be created as the last child of the target node. If you'd like to insert the new node somewhere else in relation to that target node you can use the "with relation" parameter which takes these values.

child,i
:	Insert the node at index `i` in the target's children.

previousSibling
:	Insert the node as the previous sibling of the target.

nextSibling
:	Insert the node as the next sibling of the target.

previousLine
:	Insert the node in the line before the target. Unlike "previousSibling" no adjustment will be made to the inserted node's structure level to match the target.

nextLine
:	Insert the node in the line after the target. Unlike "nextSibling" no adjustment will be made to the inserted node's structure level to match the target.

By default these commands act only on the directly referenced nodes. For example if you delete a heading then only that heading node is deleted, the headings contents are left in place. If you wish to act on the entire structure, then use the "with branch" parameter.

When you update an existing node the changes record can contain any of the above keys that are used when creating a node, and it can also contain these extras as needed.

addTags
:	Record of tag names and values that should be added to the updated node.

removeTags
:	List of tag names that should be removed from the updated node.

lineIndex
:	Index of new line position to move the node to.

parentID
:	ID of an existing node that should become the parent of the updated node.

childIndex
:	New position in parent node's children that the updated node should be moved to.

## Scripting the Editors Selection

Use "read selection" and "update selection" to read and update the editors selection. The selection is represented as a record with these keys:

text
:	Selected text.

textRange
:	Record with "location" in text where selection starts and "length".

lineRange
:	Record with "location" of first line where selection starts and "length" of total lines that it spans.

nodeRange
:	"startNode" and "startOffset" for where the selection starts and "endNode" and "endOffset" for where the selection ends.

nodeIDs
:	List of node ids that overlap the selection.

nodePath
:	Node path to all selected nodes. Useful if you want to combine the selection with some other node path, and see the results. This can be easily created from "nodeIDs" in most languages, but it's a bit painful in AppleScript, so it's precomputed here.

When updating the selection the changes record should only include one of the keys "text", "textRange", "lineRange", or "nodeRange. Also the start and end nodes must not be filtered from the view or else the selection update will fail.

This script adds a "!" after the selection:

```applescript
tell front document of application "FoldingText"
	set t to |text| of (read selection)
	update selection with changes {|text|:t & "!"}
end tell
```

## Scripting the Editors Node Path

Use "read node path" and "update node path" to read and update the node path that the editor uses to determine which nodes are visible and which are hidden in folds.

This script hides all nodes without a "@today" tag:

```applescript
tell front document of app "FoldingText"
	update node path with text "//@today"
end tell
```

## Scripting HTTP REST Style

Behind the scenes all FoldingText scripting is routed through a single "HTTP request" command. There isn't any HTTP server running in the background, but it does mimic the HTTP API style and in some future I'd like to have such a server running.

If you are already familiar with AppleScript, and you plan to write your scripts in AppleScript, then you probably don't want to use "HTTP request" directly. But if you are more familiar with HTTP then AppleScript, and want to write your scripts in Python, Ruby, or some other language, then working directly with "HTTP request" can be useful.

"HTTP request" allows you to work in the HTTP style, with JSON formatted data, and only bridge to AppleScript to make the final request. Call to the terminal using "osascript" to make the final request. For example this to gets the JSON representation of all nodes in the front document.

    osascript -e
        'tell front document
         of application "FoldingText"
         to HTTP request URI "/nodes.json"'

Many of the URIs, parameters, behaviors, and formats used by the "HTTP request" command can be guessed from the above AppleScript documentation and the AppleScript suite documentation. But to get started your best bet is to download the [syntax example scripts](/posts/extensions/scripts/syntax-examples-script.html). They demonstrate the possible URIs and how you might use them.