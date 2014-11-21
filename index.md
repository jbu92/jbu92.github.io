---
layout: page
title: JamesHaight.com
tagline: 
---
{% include JB/setup %}

## About

Howdy. Until I can figure out how to modify the navbar... Check me out on <a href="http://www.linkedin.com/in/jameshaight">LinkedIn</a> and <a href="http://github.com/jbu92">GitHub</a>.

## Recent Posts
{% for post in site.posts limit 5 %}
#### {{ post.date | date: "%B %e, %Y" }} | {{ post.title }}
{{ post.excerpt | strip_html}}<br>
            <a href="{{ post.url }}">Read more...</a><br><br>
{% endfor %}
