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

Hey look it's a website with a thing on it!

Why do I have this page? What's it for?  
Well it used to just be so I could put "me@jameshaight.com" on my resume, but now it plays host to that little badge up there that says I have a GCIA, too.  

Can I find anything useful here? (spoiler alert: no)  
My GPG key is available [here](https://keybase.io/jhaight/key.asc) if you need it for some reason.

## Recent Posts
{% for post in site.posts limit 5 %}
#### {{ post.date | date: "%B %e, %Y" }} | {{ post.title }}
{{ post.excerpt | strip_html}}<br>
            <a href="{{ post.url }}">Read more...</a><br><br>
{% endfor %}

