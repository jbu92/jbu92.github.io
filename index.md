---
layout: page
title: Welcome!
tagline: 
---
{% include JB/setup %}

## About

This will eventually be like a blog or something, I dunno... We'll figure it out together!


## Recent Posts
{% for post in site.posts limit 5 %}
<h3>{{ post.date | date: "%B %e, %Y" }} | <a href="{{ post.url }}">{{ post.title }}</a></h3>
{{ post.content | strip_html | truncatewords:75}}<br>
            <a href="{{ post.url }}">Read more...</a><br><br>
{% endfor %}
