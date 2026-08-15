---
layout: page
title: Blog
navigation_weight: 3
---

<style>
.post-header { display: none; }
</style>

> *No matter how one may think himself accomplished, when he sets out to learn a new language,
> science, or the bicycle, he has entered a new realm as truly as if he were a child newly born into the
> world.*
> 
> <cite>Frances Willard</cite>

<hr/>   

{% if site.posts.size > 0 %}
<div class="pub-list" markdown="1">
  {% for post in site.posts %}
[{{ post.title }}]({{ post.url | relative_url }})<br>
{{ post.date | date: "%B %-d, %Y" }}

  {% endfor %}
</div>
{% else %}
<p style="margin-top: 1em; color: #828282;">No posts yet, please check back soon.</p>
{% endif %}
