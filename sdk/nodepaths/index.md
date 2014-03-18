---
layout: default
title: Node Paths
---
Node paths are evaluated to match [nodes](../nodes).

You give FoldingText a node path which it then parses and evaluates and to return a list of all matching nodes. The node path syntax is loosely based on [Xpath](http://www.w3schools.com/xpath/), the hierarchical query language for XML documents.

The rest of this document describes FoldingText's node path syntax. You don't need to read it all now. Come back to it when you need a concise and powerful way to find nodes in your document.

## Paths

Simple node paths look and work like file system paths.

    /my heading/subhead

When this path is evaluated it first finds all top level nodes in the document that contain the text "my heading", and then for each matching node it returns any child that contains the text "subhead".

In the above example each step selected the next set of nodes from the children of the last matching set. This is called the "child" axis and it's the default. But you can use different syntax to search along another axis, for example:

    /my heading//subhead

The extra "/" syntax before "subhead" means that after the results from the first step are calculated then the "descendants" axis should be search instead of the "child" axis. This will find all lines that contain "subhead" in the entire "my heading" section, not just in the direct children of the "my heading" section.

The '//' operator can be used to start a node path as well. In that case, the above example would look like:

    //my heading/subhead

Now this means that instead of searching for any top level nodes, *all* nodes containing 'my heading' will be searched and only those children containing 'subhead' will be returned.

Until now, we've seen how to find nodes using filters progressively for children. However, if you know the properties of a child, and want the parent to be returned, the '..' syntax is useful. Here's an example:

    //@done/..*

This node path finds all the nodes which have at least one child node with a `@done` tag.

## Axes

Generally node paths follow the same [axis rules and syntaxes](http://www.w3schools.com/xpath/xpath_axes.asp) as defined by Xpath, but we've added one extra axis to make filtering easier. For example:

    /my heading///subhead

The '///' syntax means to start with all descendants (same as '//' syntax), but then to filter them and return the nodes that match the "subhead" filter and the ancestors of those nodes. It's a little hard to type it all out, but I think for many people it's the default behavior that they want when viewing the results of a filter–show the matches and their ancestors.

The other axes supported are:

ancestor
:	Returns all ancestors matching predicate. For example `/my heading/ancestor::*`

ancestor-or-self
:	Returns nodes and their ancestors matching predicate. For example `/my heading/ancestor-or-self::*`

descendant
:	Returns all descendants matching predicate. Same as '//' operator. For example `/my heading/descendant::*`

descendant-or-self
:	Returns nodes and their descendants matching predicate. For example `/my heading/descendant-or-self::*`

following
:	Returns all nodes following each of the previously matched nodes. For example `/my heading/following::*`

following-sibling
:	Returns sibling nodes following each of the previously matched nodes. For example `/my heading/following-sibling::*`

preceding
:	Returns all nodes preceding each of the previously matched nodes. For example `/my heading/preceding::*`

preceding-sibling
:	Returns sibling nodes preceding each of the previously matched nodes. For example `/my heading/preceding-sibling::*`

child
:	Searches child node, same as '/' operator. For example `/my heading/child::*`

parent
:	Searches parent node, same as '..' operator. For example `/my heading/parent::*`

self
:	Matches predicate on the same node. For example `/my heading/self::*`

filter-descendants
:	Filter all selected nodes based on predicate, same as '///' operator. For example `/my heading/filter-descendants::*`

**Note:** Although '/' used alone represents the 'child' axis, whenever '/' is used with an axis specification, like in:

    /NodePaths/parent::*

It only serves as a path separator, loses its 'child' axis property and adopts, instead, the explicitly stated axis i.e. 'parent' in the above example.

## Set Operations

Node paths support set operations (union, intersect, and except) for combining the results of multiple paths.

    (/Inbox//* union //@today) except //@done

The above query returns the entire contents "Inbox" combined with any line anywhere that has a "@today" tag, but it then excludes any lines that have a "@done" tag.

## Predicates

The text between each path axis is the predicate. This predicate is used to test against each node along that path axis. If no axis is specified then the default "child" axis is used.

The following examples only show predicate text. If you use them directly the default "child" axis will be used. Often that's not what you want, to match agains all nodes in your document you should put "//" at the start.

The basic form of a node path predicate is:

    @<attribute> <relation> <search term>

You don't have to specify each element in the pattern. If you don't specify something then a default value will be used. For example the following predicates are all equivalent because "line" is the default attribute and "contains" is the default relation:

    @line contains Inbox
    @line Inbox
    contains Inbox
    Inbox

This makes predicates very simple to start with, if you want to find nodes that contain inbox, just type "Inbox". But you can also be very specific with your testing and you can even combine multiple tests with boolean expressions.

If you want a fast way to match anything you can use the special form predicate "\*" which will match all nodes along the given axis without having to do any comparisons.

### Attributes

The first part of a predicate is the attribute that you want to test against. The previous examples targeted the built in "line" attribute. You can test against other built in attributes, tag attributes that you define, inline spanning attributes, and property attributes.

#### Built in Attributes

Each node has the following built in attributes:

type
:	The node type including: `heading`, `ordered`, `unordered`, `blockquote`, `codeblock`, `linkdef`, `property`, `body`, `term`, `definition`, `horizontalrule`, `empty`...

line
:	The node's entire line of text. For example you can use "@line contains joe" to match all nodes that contain the text "joe".

mode
:	Mode declared by this node. For example "@mode = todo" will return all lines that end with ".todo".

modeContext
:	Mode context effecting this node. For example "@modeContext = todo" will return the nodes that are under the influence (descendants) of a node that declares a ".todo" mode.

id
:	Each node is assigned a unique ID. These ID's are stable for the lifetime of the open document, but when you close the document they are lost. ID's aren't useful for user searches, but they can provide a useful handle to a node when doing scripting work.

property
:	Allows access to this nodes property attributes. See "Property Attributes" below.

#### Tag Attributes

You can add your own custom attributes to a node by adding tags to the node's line, and then match them like this:

To match lines with a "@priority" tag type:

    @priority

To match lines with a "@priority(1)" tag type:

    @priority = 1

#### Inline Attributes

You can also target inline spanning attributes such as **strong** and *emphasis*. You do that by adding a ":" after the built in "line" attribute and then typing the inline attribute name. For example:

To match lines with **strong** text type:

    @line:strong

To match lines with **strong** text containing "cat" type:

    @line:strong contains cat

Another useful inline spanning attribute is "text". Its value is the text content of the line excluding leading syntax and trailing tags.

### Property Attributes

You can also target properties. This allows you to match nodes based on their property node children. For example this matches all lines that have a "myproperty" property whose value is "with value":

    @property:myproperty = with value

### Relations

Relations determine the kind of test that you want the predicate to perform. Node path predicates support the following relations: "=", "!=", "<", ">", "<=", ">=", "beginswith", "endswith", and "matches" which does a regular expression match.

By default all comparisons are performed on case insensitive string values. You can change this behavior by providing a modifier after the relation like this:

    @line contains [modifier] a value

Right now four modifiers are supported which apply to all relations except for "match". Match relations are always case sensitive, but you can make them case insensitive using regular expression syntax.

i
:	Case insensitive compare, both the attribute value and the comparison value are converted to lower case before doing the compare. This is the default modifier.

s
:	Case sensitive compare, neither the attribute value or comparison value are modified before doing the compare.

n
:	Numeric compare, both the attribute value and comparison value are converted to numbers before doing the compare. So for example the values "0" and "0.0" would be equal when doing a numeric compare, but not when doing a normal string compare.

d
:	Date compare, both the attribute value and comparison value are converted to dates before doing the compare.

### Values

Most of the time you can just type the values that you are looking for and it works. However, if you want to look for something that has special meaning in the node path syntax, then you need to first enclose the value in quotes. For example to find all lines that contain an equal sign, your search must enclose the value in quotes like this:

    @line contains "="

Unless the value contains a keyword you do not need to enclose it in quotes. This is true even if the value has multiple words. For example this is perfectly valid:

    @line contains my search terms and not @done

This matches nodes that have the text "my search terms" and are not tagged with "@done".

### Boolean expressions

You can combine predicates with logical "and", "or", and "not" operators. To match nodes that contain both the text "one" and "two", or the text "three" then type:

    (one and two) or three

To match nodes that don't have the "@done" tag type:

    not @done