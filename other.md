---
layout: other
title: 其他文章
active: other
keywords: other
description: other
---

<link href="/assets/css/cates.css" rel="stylesheet">
 
<ul class="list-group list-group-flush">
   {% for post in site.categories.other %}
    <li class="list-group-item d-flex align-items-center">
        <a class="text-secondary" href="{{ post.url }}">{{ post.title }} </a>
        <span class="ml-auto date"> {{ post.date | date: "%Y-%m-%d" }} </span> 
    </li>
    {% endfor %}
</ul>
