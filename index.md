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
		<a href="{{ BASE_PATH }}{{ post.url }}#disqus_thread" data-disqus-identifier="{{ post.title }}">0 Comments</a>
		{{ post.content | split:"<!--more-->" | first }}
	</li>
	{% if forloop.index0 == 2 %}
	<li style="height:60px">
		<iframe src="//rcm-na.amazon-adsystem.com/e/cm?o=1&p=13&l=ez&f=ifr&linkID=268f3689c587481e9885f20589073720&t=bluewombat-20&tracking_id=bluewombat-20" width="468" height="60" scrolling="no" border="0" marginwidth="0" style="border:none;" frameborder="0"></iframe>
	</li>
	{% endif %}
  {% endfor %}
</ul>
<script>
$(function() {
	setTimeout(function () {
		var short_name = "bluewombat";
		var tag = document.createElement("script");
		tag.src = "https://" + short_name + ".disqus.com/count.js";
		tag.setAttribute("id", "dsq-count-scr");
		$("body").append(tag);
	}, 500);
});
</script>