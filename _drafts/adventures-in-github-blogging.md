---
layout: post
title: Adventures in GitHub Blogging
description: null
category: null
tags: []
published: true
excerpt: "Updating the BIOS, installing software, configuring things..."
---

{% include JB/setup %}

So part of the reason that this blog exists is to learn up on Markdown (specifically, Github-flavored Markdown). In that process, I've come across a number of annoying things about Jekyll and the Jekyll Bootstrap framework that I'm using to make things happen. I expect that I'll edit this post a number of times, and that the ordering of things will end up being 'wherever I felt like placing a certain item at the time that I added it.'

## Jekyll Bootstrap CSS issues with tables
Firstly, in my second post [NAS Build Part 2], I had a couple of tables in the post. By default, at least under the theme I was using at the time **add a screenshot maybe**, the table items had no borders and were all crammed together. To fix that, I ended up adding the following lines to the css file.

{% highlight css %}
/* Table Borders */
table {
  border-collapse: collapse;
}
table, th, td {
 border: solid 1px;
}
th, td{
  padding: 5px;
}
{% endhighlight %}

## Code blocks under Jekyll
While we're on the subject- you see that block of code up there? The documentation for Markdown says that to make a code block, all you have to do is surround your code with sets of three backticks (and for inline code, single backticks). The problem is, apparently Jekyll doesn't treat code blocks correctly (or maybe it's the framework or the theme, I don't know where the problem really lies), so I now have to tag my code blocks for Jekyll, instead of just being able to use a markdown codeblock **add examples n shit**