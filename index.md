---
layout: index
title: Jatai的博客
active: index
---

# {{ page.title }}
<ul>
    {% for post in site.posts %}
    <li>{{ post.date | date: "%Y-%m-%d" }} <a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
