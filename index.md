---
layout: page
title: Welcome!
tagline: 
---
{% include JB/setup %}

## About

This will eventually be like a blog or something, I dunno... We'll figure it out together!


## Recent Posts
{% for post in site.posts offset: 0 limit: 50 %}
<h4><a href="{{ post.url }}">{{ post.title }}</a></h4> | {{ post.date | date: "%B %e, %Y" }}
<p>{{ post.excerpt }}
</p>
{% endfor %}
