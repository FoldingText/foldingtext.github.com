---
layout: default
title: Plugins
---

Plugins allow you to add new features to FoldingText. This page collects useful plugins to get you started. Here are some tips for [using them](./using_plugins).

{% for post in site.categories.plugins %}

- [**{{ post.title }}**]({{ post.url }}) {{ post.description | strip_html }}

{% endfor %}