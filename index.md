---
layout: page
title: It's a blog!
tagline: 
---
{% include JB/setup %}

## About

Welcome to my blog. It should primarily be about computers and stuff, and exists almost entirely for my own future reference.


## Recent Posts
{% for post in site.posts limit 5 %}
#### {{ post.date | date: "%B %e, %Y" }} | {{ post.title }}
{{ post.excerpt | strip_html}}<br>
            <a href="{{ post.url }}">Read more...</a><br><br>
{% endfor %}
