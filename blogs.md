---
layout: nav
title: 博客
permalink: /blogs/
---
### 博客
{% for post in site.posts %}
<a href="{{ post.url | relative_url }}">{{ post.title }}</a>
<span class="post-meta">{{ post.date | date: "%Y-%m-%d"}}</span>
{% endfor %}


