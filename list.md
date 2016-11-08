---
layout: list
title: 文章列表
active: list
---

文章列表
<ul>
    {% for post in site.posts %}
    <li>{{ post.date | date: "%Y-%m-%d" }} <a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
