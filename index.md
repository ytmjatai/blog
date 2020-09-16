---
layout: index
title: 首页
active: index
keywords: jatai
description: 本博客主要记录博主平时的工作笔记，及关于前端技术(如:javascript、angularjs、node、webpack、gulp、dojo、jquery、css3、html5)的一些简单教程
---

<link href="/assets/css/index.css" rel="stylesheet">

<ul class="list-group list-group-flush index">
   {% for post in site.posts %}
    <li class="list-group-item d-flex align-items-center">
        <div>
            <a class="text-secondary" href="{{ post.url }}">
               <h4> {{ post.title }}</h4>
            </a>
            <p class="mt-3 mb-0"> {{post.intro}}  <a class="ml-3 text-primary" href="{{ post.url }}">查看详情</a></p>
        </div>
        <span class="ml-auto date"> {{ post.date | date: "%Y-%m-%d" }} </span> 
    </li>
    {% endfor %}
</ul>