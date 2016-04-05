---
layout: page
title: JamesHaight.com
tagline: 
---
{% include JB/setup %}

<div data-iframe-width="250" data-iframe-height="270" data-share-badge-id="d0b25cc0-980d-40fd-8765-b9183dcd24b2"></div>
  <script type="text/javascript">
    (function() {
      var s = document.createElement('script');
      s.type = 'text/javascript';
      s.async = true;
      s.src = '//www.youracclaim.com/assets/utilities/embed.js';
      var o = document.getElementsByTagName('script')[0];
      o.parentNode.insertBefore(s, o);
      })();
  </script>
  
## About

Hey check it out it's a thing. GPG key available [here](https://keybase.io/jhaight/key.asc) if you need it for some reason.

## Recent Posts
{% for post in site.posts limit 5 %}
#### {{ post.date | date: "%B %e, %Y" }} | {{ post.title }}
{{ post.excerpt | strip_html}}<br>
            <a href="{{ post.url }}">Read more...</a><br><br>
{% endfor %}

