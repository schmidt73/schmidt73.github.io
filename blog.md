---
layout: page
title: Blog
navigation_weight: 3
---

<style>
.post-header { display: none; }
.pub-list { font-size: 1.15em; }
.pub-list p a { font-size: 0.9em; }
.blog-post-marker { font-size: 0.75em; }
.blog-post-date { font-size: 0.75em; }
.post-content blockquote p { line-height: 1; }
.post-content blockquote p:first-child { margin-bottom: 10px; }
</style>

> *No matter how one may think himself accomplished, when he sets out to learn a new language,
> science, or the bicycle, he has entered a new realm as truly as if he were a child newly born into the
> world.*
> 
> <cite>Frances Willard</cite>

{% if site.posts.size > 0 %}
<div style="margin-top: 1em;" class="pub-list" markdown="1">
  {% for post in site.posts %}
<span class="blog-post-marker" aria-hidden="true">◆</span> [<span style="color:#c869bf">{{ post.title }}</span>]({{ post.url | relative_url }})<br>
<span class="blog-post-date">{{ post.date | date: "%B %-d, %Y" }}</span>

  {% endfor %}
</div>
{% else %}
<p style="margin-top: 1em; color: #828282;">No posts yet, please check back soon.</p>
{% endif %}
