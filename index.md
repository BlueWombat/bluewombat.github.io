---
layout: page
title: BlueWombat's Space
tagline: My ramblings, projects etc.
---
{% include JB/setup %}

Below you'll find a series of blog posts. I write about everything I find interesting, daily work challenges, random thoughts and really everything and nothing.
I write this blog both for you to enjoy, and as a reminder of useful approaches for myself.
I hope you'll enjoy it as much as I enjoy writing it; have fun :-)
<br /><br />

<ul class="posts">
  {% for post in site.posts %}
	<li>
		<a href="{{ BASE_PATH }}{{ post.url }}"><h2>{{ post.title }}</h2></a>
		{{ post.content | split:"<!--more-->" | first }}
	</li>
  {% endfor %}
</ul>