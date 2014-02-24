---
layout: default
title: Create Themes (FoldingText 2.0 Dev Only)
---
Use themes to change colors, fonts, and other aspects of how FoldingText looks. You should first understand:

- [Nodes](../nodes)
- [CSS/LESS](http://www.lesscss.org/)
- [SDK Runner](../runner)

## About

The theme API is open and messy.

FoldingText's editor is implemented in JavaScript/HTML/DOM. Your themes can style any aspect of the DOM that they wish. This gives you a lot of power, but there's limited documentation and no backward compatibility guarantees other then me doing my best to keep existing themes.

## Getting Started

1. Choose the menu item File > Open Application Folder

2. Inside that folder create "user.less" with this content:

```less
@textColor: red;
```

That's your first theme! Make a new FoldingText document and the text should be red.

To create more complex themes:

1. Use the SDK Runner for a live preview of your changes.

2. Click the SDK Runner's "Run Editor" link and then click the "Inspector" toolbar item to view the editor's HTML DOM structure so you know what you can style.

3. Click the SDK Runner's "SDK Sources" link to reveal the "sdk" folder. And then look at the ".less" files in the "sources" folder to see the default style rules that are used.

4. Ask questions in the [Extensions](http://support.foldingtext.com/discussions/extensions) support forum.