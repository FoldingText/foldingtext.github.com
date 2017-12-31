---
layout: default
title: Blog
---

{% for post in site.posts %}

# [**{{ post.title }}**]({{ post.url }})
_{{ post.date }}_

{{ post.content }}

{% endfor %}