---
layout: default
title: Blog
---

# Blog Posts

<h2>Deep Learning</h2>
<ul class="blog-listing">
{% for post in site.posts %}
    {% if post.categories contains 'Deep Learning' %}
    <li><a href="{{ post.url | absolute_url }}" target="_blank">{{ post.title }}</a> - {{ post.date | date: "%b %-d, %Y" }}</li>
    {% endif %}
{% endfor %}
</ul>

<h2>Other Categories</h2>
<ul class="blog-listing">
{% for post in site.posts %}
    {% unless post.categories contains 'Deep Learning' %}
    <li><a href="{{ post.url | absolute_url }}" target="_blank">{{ post.title }}</a> - {{ post.date | date: "%b %-d, %Y" }}</li>
    {% endunless %}
{% endfor %}
</ul>

