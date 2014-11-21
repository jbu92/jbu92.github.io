---
layout: page
title: JamesHaight.com
tagline: 
---
{% include JB/setup %}

## About

Howdy. I really need to come up with something to put here.

## Recent Posts
{% for post in site.posts limit 5 %}
#### {{ post.date | date: "%B %e, %Y" }} | {{ post.title }}
{{ post.excerpt | strip_html}}<br>
            <a href="{{ post.url }}">Read more...</a><br><br>
{% endfor %}
