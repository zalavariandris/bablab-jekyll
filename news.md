---
title: News
layout: default
---

# news

<ul>
{% for post in site.posts %}
	<li>
		<a href="{{post.url}}">{{post.title}}</a>
	</li>
{% endfor %}
</ul>