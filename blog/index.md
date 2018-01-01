---
layout: default
title: Blog
---

{% for post in site.posts %}

## [**{{ post.title }}**]({{ post.url }})
_{{ post.date | date: "%B %-d, %Y" }}_

{{ post.content }}

## ***

{% endfor %}

<!— Pagination links —>
<div class="pagination">
  {% if paginator.previous_page %}
    <a href="{{ paginator.previous_page_path }}" class="previous">Previous</a>
  {% else %}
    <span class="previous">Previous</span>
  {% endif %}
  <span class="page_number">Page: {{ paginator.page }} of {{ paginator.total_pages }}</span>
  {% if paginator.next_page %}
    <a href="{{ paginator.next_page_path }}" class="next">Next</a>
  {% else %}
    <span class="next">Next</span>
  {% endif %}
</div>
