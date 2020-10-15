---
layout: python
title: python
active: python
keywords: python
description: python
---

<link href="/assets/css/cates.css" rel="stylesheet">
 
<ul class="list-group list-group-flush">
   {% for post in site.categories.python %}
    <li class="list-group-item d-flex align-items-center">
        <a class="text-secondary" href="{{ post.url }}">{{ post.title }} </a>
        <span class="ml-auto date"> {{ post.date | date: "%Y-%m-%d" }} </span> 
    </li>
    {% endfor %}
</ul>
