# Frequently Asked Questions

FoldingText is an extensible markdown text editor with productivity features. It’s most easily understood by watching this [Demo Movie](https://foldingtext.com/#watchmovie)

- Markdown Editor
- Extensible with themes, scripts and plugins
- Outlining, todos, timers for productivity

## How can I create my own themes, scripts and plugins?

To create your own themes, scripts, and plugins open FoldingText’s Help -> Software Development Kit menu item then from there you can access the SDK documentation.

## How to install a Theme?

Themes are made from CSS/LESS rules that you put in your user.less file. To use a theme open FoldingText based app and:

1. Choose the File > Open Application Folder menu item.
2. Inside that folder open your existing user.less text file, or create a new one if none exists.
3. Paste in the theme extensions CSS/LESS rules and save.
4. Create a new document to see the theme applied.

## How to install a Script?

Scripts are made of AppleScript code that you paste into the “AppleScript Editor” application to run

**To try a script:**

1. Create a new test document in FoldingText.
2. Open the “AppleScript Editor” app and paste in the script.
3. Then press the green “Run” button to run the script.

If you decide to keep the script you should install it so that it’s easier to run.

**To install a script:**

   1. Open the "AppleScript Editor" apps preferences and make sure that "Show Script menu in menu bar" is checked.
   2. Click on that item in your menu bar and choose “Open Scripts Folder > Open User Scripts Folder”.
   3. Save the script that you were testing into that folder. Make sure to save using the “Script” format, it’s faster and some scripts wont work properly unless they are saved in “Script” format.

Once you’ve done this the script will be listed in the Script menu in your menu bar. To run the script just select it from that menu. You can also use tools such as [FastScripts](https://www.red-sweater.com/fastscripts/) and [Keyboard Maestro](https://www.keyboardmaestro.com/main/) to setup keyboard shortcuts to launch scripts.

## How to install a plugin?

    Plugins are folders that end with the .ftplugin file extension and contain JavaScript and other resources.

    To install a plugin put it into FoldingText’s Plug-In’s folder:

   1. Choose the File > Open Application Folder menu item and then locate the Plug-Ins folder.
   2. Copy the plugin (which is a folder ending with .ftplugin) into the Plug-Ins folder.
   3. Verify that a plugin is installed by choosing the FoldingText -> Plugin Manager menu item and make sure the plugin is install and has no errors. The plugin functionality should be available in any new documents that you open after installing the plugin.

## What parts of Multi-markdown syntax does FoldingText support?

First of all FoldingText is a plain-text editor, it can open any plain text file (of reasonable size) no matter what the formatting.

So by “support” I don’t mean opening and editing (it can do that with any plain text file) I mean that FoldingText will recognize the given syntax and highlight it to make it easier to read. Also note that FoldingText doesn’t have any special export of multi-markdown to other formats. For that you’d need to use [Multimarkdown Composer](http://multimarkdown.com/), though you could of course still use FoldingText to edit the file.

So with that definition of “supports”, what parts of Multi-markdown syntax does FoldingText support and what parts does it not support?

Syntax highlighting supported:

- definition lists

Syntax highlighting partially supported

- footnotes
- image attributes
- citations and bibliography (works best in LaTeX using BibTeX)

Syntax highlighting unsupported:

- tables
- math support
- table and image captions
- document metadata(e.g. title, author, etc)

In the release notes for FoldingText 2.0 I said supports “large parts of MultiMarkdown”. I’m not sure how I started thinking that way, but it’s not true. “Some” parts is more correct.

## How to change keybindings?

### FoldingText has two kinds of keybindings

1. The standard “OS X menu item” shortcuts. These can be changed in System Preferences as [Described Here](https://support.apple.com/kb/ph3957)
2. Internally defined keybindings. These take precedent over the “OS X menu item” shortcuts, and can be changed with a plugin.

To change an internal keybinding you need to create a plugin that adds the new binding. For example lets say you want to rebind the Home and End keys to move to the start and end of the current line instead of to scroll to the top or bottom of your document, which is the default behavior. To do this you will:

1. [Create a plugin](#How-can-I-create-my-own-themes,-scripts and-plugins?) Here is [an example plugin](https://github.com/FoldingText/plugins/tree/master/keybindings.ftplugin) that shows in particular how to add new keybindings.

2. Modify that plugin to override the bindings you are interested in. In this case (changing Home and End behavior) the plugin code should be modified to this:

```javascript
    editor.addKeyMap({
        'Home' : 'moveToBeginningOfLine',
        'End' : 'moveToEndOfLine'
    });
 ```

Modifiers can be combined with the bound keys with:

```shell
   Shift-, Cmd-, Ctrl-, and Alt- (in that order)
```

3. Then [install the plugin](#how-to-install-a-plugin) if you haven't already done so. And create a new document, the bindings should be active in the new document.

## Exploring FoldingText’s internal keybindings

If you are modifying keybindings it can be very useful to view FoldingText’s default keybindings so you know what you are overriding. It’s possible, but you’ll need to do some digging.

FoldingText ships with most of it’s own source code embedded inside the application bundle (not open source, but shared source so you can learn from it). Anyway to see FoldingText’s existing keybindings:

1. Right click on the FoldingText.app bundle and choose “Show Package Contents”.
2. Then browse into that folder and find the keybindings file at FoldingText.app/Contents/Resources/dist/sdk/sources/ft/editor/keybindings.js.
3. Open that file to see FoldingText’s default text editing bindings. (There are a few others spread out through the code (you can search for addKeyMap), but that’s the vast majority of them.

### How to change the default file extension

1. You need to open “Terminal” app paste in something like this:

   `defaults write com.doubledogsoftware.FoldingText DefaultFileExtension [newextension]`

That’s setting a user default for the application with the identifier: com.doubledogsoftware.FoldingText.

### Will FoldingText be availble on other platforms?

My focus is on macOS and iPad for the foreseeable future.
