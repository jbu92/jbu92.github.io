---
layout: page
title: JamesHaight.com
tagline: 
---
{% include JB/setup %}

## About

Hey check it out it's a thing. GPG key available [here](https://keybase.io/jhaight/key.asc} if you need it for some reason.

## Recent Posts
{% for post in site.posts limit 5 %}
#### {{ post.date | date: "%B %e, %Y" }} | {{ post.title }}
{{ post.excerpt | strip_html}}<br>
            <a href="{{ post.url }}">Read more...</a><br><br>
{% endfor %}

