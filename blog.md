---
layout: default
title: Blog
---

# Blog Posts

## Deep Learning
{% for post in site.posts %}
        {% if post.categories contains 'Deep Learning' %}
        <li><a href="{{ post.url | absolute_url }}" target="_blank">{{ post.title }}</a> - {{ post.date | date: "%b %-d, %Y" }}</li>
        {% endif %}
{% endfor %}
    
## Other Categories
{% for post in site.posts %}
        {% if post.categories contains 'Others' %}
        <li><a href="{{ post.url | absolute_url }}" target="_blank">{{ post.title }}</a> - {{ post.date | date: "%b %-d, %Y" }}</li>
        {% endif %}
{% endfor %}
    