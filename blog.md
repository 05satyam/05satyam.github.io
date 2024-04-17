---
layout: default
title: Blog
---

# Blog Posts

## Neural Networks

### Distance Metrics in Machine Learning
How different distance metrics play a crucial role in machine learning:
- **[Read More](/blogs/distance-metrics-in-machine-learning)**

### MLP: Multilayer Perceptron
The simplest form of deep neural networks: Multilayer Perceptron
- **[Read More](/blogs/mlp-multilayer-perceptron)**

## Large Language Models

### Exploring Large Language Models

### Data Analysis and Text Summarization (DATS)
- **[GitHub Repository](https://github.com/05satyam/DATS)**

<ul class="blog-listing">
{% for post in site.posts %}
    <li><a href="{{ post.url | absolute_url }}" target="_blank">{{ post.title }}</a> - {{ post.date | date: "%b %-d, %Y" }}</li>
{% endfor %}
</ul>
