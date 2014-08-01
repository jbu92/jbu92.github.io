---
layout: page
title: Welcome!
tagline: 
---
{% include JB/setup %}

## About

This will eventually be like a blog or something, I dunno... We'll figure it out together!

<div class="blog-index">
	{% assign post = site.posts.first %}
	{% assign content = post.content %}
	{% include post_detail.html %}
</div>
