---
layout: index
title: 首页
active: index
keywords: jatai,博客,javascript,angularjs,node,webpack,gulp,dojo,jquery,css3,html5
description: 本博客主要记录博主平时的工作笔记，及关于前端技术(如:javascript、angularjs、node、webpack、gulp、dojo、jquery、css3、html5)的一些简单教程
---

# {{ page.title }}
<ul>
    {% for post in site.posts %}
    <li>{{ post.date | date: "%Y-%m-%d" }} <a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
