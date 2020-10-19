--- 
layout: article 
title: Django REST framework 入门与项目模块化
tags: python django
categories: python 
keywords: python, django, Django REST framework, rest_framework 模块化
description: python 学习, django, Django REST framework, rest_framework 模块化
intro: 本来入门 Django 后准备放弃的, 但是后来接触了 Django REST framework, 发现用它写 REST API 实在是太爽了, 所以不但没有弃坑, 而且发现有越陷越深的趋势...
---

# {{page.title}}

----
#### 引言 ####

本来入门 Django 后准备放弃的, 但是后来接触了 Django REST framework, 发现用它写 REST API 实在是太爽了, 所以不但没有弃坑, 而且发现有越陷越深的趋势...

下面来记录一下 Django REST framework 的入门与用它实现项目的模块化

----
#### 安装 Django REST framework 与配置 ####

这玩意没啥技术含量, 跟着 <a href="https://www.django-rest-framework.org/#installation" target="_blank" title='Django REST framework官网'>Django REST framework 官方网站</a> 教程走一遍应该不会有问题

<abc>bin/bash</abc>
```bash
  pip install djangorestframework
```
<abc>config/settings.py</abc>
```python
  INSTALLED_APPS = [
    ...
    'rest_framework',
]
```
----
#### 项目模块化 ####

看着官网的例子, 什么 模块定义/路由设置 都搞到一个文件去了, 正式的项目不会那么搞.

我们可以把 Model, Serializer, ViewSet 分别定义在不同的文件夹目录下, 因为随着项目的扩展, 
用到的数据表会越来越多; 也可以把同一个数据表的 Model/Serializer/ViewSet 定义在同一个文件下,
一个数据表对应一个文件, 不用翻来翻去的到处找

当然项目模块化和 Django REST framework 没啥关系, 每一个好的 Python 项目, 甚至每一个其他语言
的程序项目都应该采用合理的结构化

Python 模块和包的知识就不展开介绍了, 记得在新建的 models/ 目录下新建 \_\_init\_\_.py 文件

<abc>library_system/models/book.py</abc>

```python
from django.db import models
from rest_framework import serializers, viewsets

class Book(models.Model):
    ISBN = models.CharField(max_length=200)
    name = models.CharField(max_length=200)
    author = models.CharField(max_length=200)
    summary = models.CharField(max_length=200)
    categoryId = models.CharField(max_length=200)
    thumbnail = models.CharField(max_length=200)
    pictures = models.CharField(max_length=200)

class BookSerializer(serializers.HyperlinkedModelSerializer):
  class Meta:
    model = Book
    fields = ['ISBN', 'name', 'author', 'summary', 'categoryId', 'thumbnail', 'pictures' ]


class BookViewSet(viewsets.ModelViewSet):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
```
----



#### 结语 ####
又是快乐的一天.

---