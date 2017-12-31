---
layout: default
title: Blog
---

{% for post in blog.posts %}

## [**{{ post.title }}**]({{ post.url }})

{{ post.description | strip_html }}

{% endfor %}