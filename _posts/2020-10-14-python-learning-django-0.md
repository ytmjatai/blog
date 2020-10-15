--- 
layout: article 
title: Python 之学习 Django 从入门到放弃
tags: python django
categories: python 
keywords: python, django
description: python 学习, django
intro: Python 的虚拟环境是什么? requirements.txt 是干嘛用的? 如何管理与安装项目依赖? 为什么我运行不了别人的 django 项目? 我写的 django 项目发布到服务器上? 一个正式的 django 项目第一步应该怎么做? ... 
---

# {{page.title}}

----
##### 引言 #####
Python 的虚拟环境是什么? requirements.txt 是干嘛用的? 如何管理与安装项目依赖? 为什么我运行不了别人的 django 项目? 我写的 django 项目发布到服务器上? 一个正式的 django 项目第一步应该怎么做? ... 

网络上一堆 python 和 django 的教程, 有教你如何写 Hello world, 有教你如何创建第一个项目的, 一顿操作猛如虎后, 发现弄出来的项目放到服务器上用不了, 在别人电脑上也运行不起来. 

这些文章没有从实际项目出发教你有用的第一步, 我也是自己摸索了好久, 一步一步查资料, 不断地尝试与努力学习, 终于从一个懵懂的小白, 成长为了现在一脸懵逼的小白. 

当然我也不会跟你讲上面的那些问题, 我只是在写我的学习日志.

----
##### 安装虚拟环境 #####

<abc>bin/bash</abc>
```bash
  python3 -m venv venv-name
  cd venv-name
  source bin/active
  pip install django
```
----
##### 新建项目 #####

<abc>bin/bash</abc>
```bash
django-admin startproject config
mv config app
cd app
python manage.py runserver
```
----

##### 创建应用及安装依赖 #####
创建应用及安装依赖, 配置 mysql 数据库
<abc>bin/bash</abc>
```bash
python manage.py startapp library_system
```

<abc>config/settings.py</abc>
```bash
INSTALLED_APPS = [
    ...
    'library_system.apps.LibrarySystemConfig',
]
...
DATABASES = {
    'default': {
         'ENGINE': 'django.db.backends.mysql',
        'NAME': 'library_system',
        'USER': 'root',
        'PASSWORD': '123456',
        'HOST': '127.0.0.1',
        'PORT': '6666',
    }
}
```

<abc>config/__init__.py</abc>
```bash
import pymysql
pymysql.version_info = (2, 0, 1, "final", 0)
pymysql.install_as_MySQLdb()
```


<abc>bin/bash</abc>
```bash
pip install mysqlclient
pip install pymysql
python manage.py makemigrations
python namage.py migrate
python manage.py runserver
```
----

<abc>bin/bash</abc>
```bash
pip freeze > requirements.txt
// 迁移到其他环境执行安装依赖命令
pip install -r requirements.txt
```
----


##### 结语 #####
至此, 一个 Django 版的图书管理系统框架就出来了

---