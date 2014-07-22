---
layout: page
title: BlueWombat's Space
tagline: My ramblings, projects etc.
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
    <a href="{{ BASE_PATH }}{{ post.url }}"><h2>{{ post.title }}</h2></a>
	{{ post.content | split:"<!--more-->" | first }}
  {% endfor %}
</ul>