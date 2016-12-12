---
layout: page
title: Spooky Stuff
tagline: Musings of a Machine Learning Engineer
---
{% include JB/setup %}

<h3>Latest Updates</h3>

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
