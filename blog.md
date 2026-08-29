---
layout: page
title: Blog
navigation_weight: 3
---

<style>
.post-header { display: none; }
.pub-list { font-size: 1.15em; }
.pub-list p a { font-size: 0.9em; }
</style>

> *No matter how one may think himself accomplished, when he sets out to learn a new language,
> science, or the bicycle, he has entered a new realm as truly as if he were a child newly born into the
> world.*
> 
> <cite>Frances Willard</cite>

<hr/>   

{% if site.posts.size > 0 %}
<div style="margin-top: 1em;" class="pub-list" markdown="1">
  {% for post in site.posts %}
[<span style="color:#c869bf">{{ post.title }}</span>]({{ post.url | relative_url }})<br>
{{ post.date | date: "%B %-d, %Y" }}

  {% endfor %}
</div>
{% else %}
<p style="margin-top: 1em; color: #828282;">No posts yet, please check back soon.</p>
{% endif %}
