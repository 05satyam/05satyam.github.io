---
layout: default
title: Blog
---

# Blog Posts

<h2>Deep Learning</h2>
<ul class="blog-listing">
{% for post in site.posts %}
    {% if post.categories contains 'DeepLearning' %}
    <li><a href="{{ post.url | absolute_url }}" target="_blank">{{ post.title }}</a> - {{ post.date | date: "%b %-d, %Y" }}</li>
    {% endif %}
{% endfor %}
</ul>

<h2>Pytorch</h2>
<ul class="blog-listing">
{% for post in site.posts %}
    {% if post.categories contains 'PyTorch' or if post.categories contains 'ML'%}
    <li><a href="{{ post.url | absolute_url }}" target="_blank">{{ post.title }}</a> - {{ post.date | date: "%b %-d, %Y" }}</li>
    {% endif %}
{% endfor %}
</ul>

<h2>Generic</h2>
<ul class="blog-listing">
{% for post in site.posts %}
    {% if post.categories contains 'Others' %}
    <li><a href="{{ post.url | absolute_url }}" target="_blank">{{ post.title }}</a> - {{ post.date | date: "%b %-d, %Y" }}</li>
    {% endif %}
{% endfor %}
</ul>

## .ipynb-Practice Notebooks
 - [RAG-OpenAIEmbeddings-PineconeDB](https://github.com/05satyam/large_language_models/blob/main/rag/rag_openai_embedding_and_pinecone.ipynb)
- [RAG-With-Langchain](https://github.com/05satyam/large_language_models/blob/main/rag/rag_with_langchain.ipynb)
- [RAG-Hybrid Search](https://github.com/05satyam/large_language_models/blob/main/rag/HybridSearch.ipynb)
- [Basic: LoRA-Finetuning](https://github.com/05satyam/large_language_models/blob/main/Simple_LoRA.ipynb)
- [Semantic Search With Pinecone ](https://github.com/05satyam/large_language_models/blob/main/Semantic_Search_With_Pinecone.ipynb)
- [ML-Model design steps(starting-2-end) with simple Linear Regression](https://github.com/05satyam/blogs/blob/main/PredictionModelDesignWithStepAndExample.ipynb)
