---
layout: default
title: Nodes
---
Nodes are FoldingText's internal data structure.

When FoldingText reads text it creates a node for each line. Each node is then classified based on it's text content and the content of surrounding nodes.

For example when FoldingText sees:

```
# My heading
- item one
- item two
```

It creates this (simplified) node structure:

- **node**: "# My heading", **type**: "heading"
	- **node**: "- item one", **type**: "unordered"
	- **node**: "- item two", **type**: "unordered"

Each node is assigned a "type" and each node is positioned in a parent/child hierarchy in relation to the other nodes. In this case the first line is classified as a heading, and the other two lines are classified as unordered children of that heading.

Nodes have other properties including a unique "id", list of associated "tags", and many others. But the important thing to remember about nodes is that they correspond 1 to 1 with lines in your document, and they are used to give each line a location and type in FoldingText's internal node tree document structure.

Use [node paths](../nodepaths) to find nodes.