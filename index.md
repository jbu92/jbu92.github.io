---
layout: page
title: Welcome!
tagline: 
---
{% include JB/setup %}

## About

This will eventually be like a blog or something, I dunno... We'll figure it out together!

<article>
<h1 class="title">Blog Archive</h1>
<section>
{% for post in site.categories.blog %}
{% capture post_month %}
{{post.date | date: "%m"}}
{% endcapture %}
{% if post_month != prev_post_month %}
{% if prev_post_month %}
<br><!-- close month -->
{% endif %}
<b>{{post.date | date: "%B %Y"}}</b><br><!-- br is open month -->
{% endif %}
<a href="{{ post.url }}">{{ post.title }}</a> <time>{{post.date | date: "%A %e" }}</time><br>
{% capture prev_post_month %}{{post_month}}{% endcapture %}
{% endfor %}
</section>
</article>
