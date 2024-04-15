---
layout: default
title: Blog
---

# Blog Posts

<ul class="blog-listing">
{% for post in site.posts %}
    <li><a href="{{ post.url | absolute_url }}">{{ post.title }}</a> - {{ post.date | date: "%b %-d, %Y" }}</li>
{% endfor %}
</ul>
