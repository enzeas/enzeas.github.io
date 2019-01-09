---
layout: nav 
title: 分类
permalink: /categories/
---
{% assign sort_categories =  site.categories | sort %}
{% for category in sort_categories %}
- <a href="#{{ category | first }}">{{ category | first }}</a>
{% endfor %}

{% for category in sort_categories %}
###  <a name="{{category | first }}" class="anchor">{{ category | first }}</a>
  {% assign sorted_posts = category.last | sort %}
	{% for post in sorted_posts %}
<a href="{{ post.url | relative_url }}">{{ post.title }}</a>
<span class="post-meta">{{ post.date | date: "%Y-%m-%d"}}</span>
	{% endfor %}
{% endfor %}
