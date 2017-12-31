---
layout: default
title: Blog
---

{% for post in site.posts %}

## [**{{ post.title }}**]({{ post.url }})

{{ post.description | strip_html }}

{% endfor %}