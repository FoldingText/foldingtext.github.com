---
layout: default
title: Blog
---

{% for post in site.posts %}

## [**{{ post.title }}**]({{ post.url }})
_{{ post.date | date: '%B %d, %' }}_

{{ post.content }}

## ***

{% endfor %}
