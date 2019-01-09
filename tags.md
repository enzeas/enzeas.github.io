---
layout: nav
title: 标签
permalink: /tags/
---
{% assign sort_tags =  site.tags | sort %}
{% for tag in sort_tags %}
- <a href="#{{ tag | first }}">{{ tag | first }}</a>
{% endfor %}

{% for tag in sort_tags %}
###  <a name="{{tag | first }}" class="anchor">{{ tag | first }}</a>
  {% assign sorted_posts = tag.last | sort %}
	{% for post in sorted_posts %}
<a href="{{ post.url | relative_url }}">{{ post.title }}</a>
<span class="post-meta">{{ post.date | date: "%Y-%m-%d"}}</span>
	{% endfor %}
{% endfor %}
