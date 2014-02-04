---
layout: default
title: Create Plugins (FoldingText 1.3 Dev Only)
---
Use plugins to integrate directly with FoldingText's code and add new features. You should first understand:

- [Nodes](../nodes)
- [Node Paths](../nodepaths)
- [SDK Runner](../runner)

## About

The JavaScript plugin API is open and messy.

FoldingText's entire JavaScript codebase is available to use as a reference (not open source) when developing your extensions. You have the same view of the system that I have and your extensions can use all the same API's.

That gives you a lot of power! But there is limited documentation and no backward compatibility guarenteess. I'll do my best to keep things stable (write specs for you plugin to help), but if you are looking for lots of documentation and backward compatibility promises I would recommend avoiding plugin development.

## Getting Started

1. Choose the menu item File > Open Application Folder

2. Make a new folder with the extension ".ftplugin" inside the "Plug-Ins" folder.

3. Inside your plugin's folder create "main.js" with this content:

```javascript
define(function(require, exports, module) {
	var Extensions = require('ft/core/extensions');
	Extensions.add('com.foldingtext.editor.init', function (editor) {
		editor.addKeyMap({
			'Cmd-H' : function (editor) {
				editor.replaceSelection('Hello world!');
			}
		});
	});
});
```

That's your first plugin! Now "Command-H" inserts "Hello world!" instead of hiding FoldingText.

To create more complex plugins:

1. Read through the rest of this document for a high level overview of FoldingText's code.

2. Learn from the existing [plugins](/posts/extensions/plugins/) and use them as starting points for your own solutions.

3. Develop using the SDK Runner for live refresh, to set breakpoints, and to step into FoldingText's uncompressed code.

4. Click the SDK Runner's "SDK Sources" link to browse FoldingText's uncompressed source code. See how the internals work, and look for places where your plugin can join the fun.

5. Ask questions in the [Extensions](http://support.foldingtext.com/discussions/extensions) support forum.

## Open and Messy

FoldingText plugin development is open and messy. The entire codebase is exposed to plugins, you have the same view of the system that I do. There is limited documentation, lots of mess, and no backward compatibility promises.

How can this work?

My hope is that it will go something like this:

1. We'll get some early adopters developing plugins. They will find problems. I'll fix problems. They will ask questions, I'll try to answer questions. This will go on for some time.

2. The codebase and the documentation will improve. We'll learn what kinds of things are likely to change and cause breakage. We'll build up a set of well done plugins examples.

3. Popular plugins will take part in FoldingText's testing system by including a "spec.js" file so that I can easily test that my changes don't break existing plugins. And when they do need to break I'll be able to quickly notify plugin developers.

## Code Layout

The code that plugins care about is in:

    sdk/sources/ft

core
:	The core classes. These are classes and methods that your plugin should generally be using.

extensions
:	Extensions to FoldingText's core behavior. This is a good place to look for ideas on how your own plugins might plug into and extend FoldingText.

modes
:	Modes included with FoldingText. Good templates for creating your own modes.

taxonomy
:	Generally plugins should ignore.

textviews
:	Implementation detail, plugins should ignore.

until
:	Utility classes, use as needed.

## 10,000 Feet Up

FoldingText's basic purpose is to map lines of text into a tree of nodes. To maintain this tree as content is updated. To allow text editing. And to allow extensions. This is how that happens:

1. "node.js" and "tree.js" are used to store and manipulate the node tree structure.

2. Taxonomies "taxonomy.js" and subclasses are used to classify nodes (their types, structure level, tags, etc) based on the nodes raw text content. Taxonomies define the underlying document syntax.

3. The "classifier.js" manages the process of applying taxonomy rules to create a properly classified node tree. This process doesn't necessarily happen all at once. So it can be the case that part of your document is properly classified, and other parts have not yet caught up. Use the "ensureClassified" method to make sure that a certain range, or entire document, is fully classified.

4. "editor.js" is the text editor. It manages things like the text selection, performs commands, and lots of pretty standard text editor things. Behind the scenes much of the editor is implemented using [CodeMirror](http://www.codemirror.net). But that should be an implementation detail that plugins should not rely on.

5. "extensions.js" is used to register extensions with extensions points such as the "com.foldingtext.editor.init" extension point that's used in the above example. There are lots of other extension points in the code. If you search for "processExtensions" you can see each extension point that's defined and how it processes it's extensions. Also search for "Extensions.add" to see examples of how to extend that point.