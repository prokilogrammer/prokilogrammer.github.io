---
layout: page
title: Mokkai
excerpt: "An archive of Mokkai posts sorted by date."
---

<ul class="post-list">
{% for post in site.categories.mokkai %} 
  <li><article><a href="{{ site.url }}{{ post.url }}">{{ post.content }} <span class="entry-date"><time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%B %d, %Y" }}</time></span></a></article></li>
{% endfor %}
</ul>
