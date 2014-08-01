---
layout: page
title: Welcome!
tagline: 
---
{% include JB/setup %}

## About

This will eventually be like a blog or something, I dunno... We'll figure it out together!

{% for post in site.posts offset: 0 limit: 50 %}
<h4><a href="{{ post.url }}">{{ post.title }}</a></h4>
        <p>
          {{ post.summary }}
        </p>
<p>
          <i class="icon-calendar"></i> {{ post.date | date: "%B %e, %Y" }}
{% endfor %}
