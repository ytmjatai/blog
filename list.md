---
layout: list
title: 全部文章
active: list
---

<link href="/assets/css/index.css" rel="stylesheet">
 
<ul class="list-group list-group-flush">
   {% for post in site.posts %}
    <li class="list-group-item d-flex align-items-center">
        <a class="text-secondary" href="{{ post.url }}">{{ post.title }} </a>
        <span class="ml-auto date"> {{ post.date | date: "%Y-%m-%d" }} </span> 
    </li>
    {% endfor %}
</ul>
