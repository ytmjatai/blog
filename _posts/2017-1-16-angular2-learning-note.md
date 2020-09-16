--- 
layout: article 
title: angular2学习笔记1-开发环境
tags: angular
categories: angular
keywords: angular2,ng-bootstrap,在组件中用ngModal打开另一个组件
description: angular2学习笔记，记录学习angular2遇到的小坑及注意事项
intro: 最近的项目都是用dojo来写，有一段时间没写angular代码了，貌似很多东西都忘记了，主要是基础不牢固，现在angular2都已经出来了，果断投入angular2的怀抱，趁着年底没项目做，前几天开始学习angular2，在这里写下学习angular2的笔记
---
# {{page.title}}

### 快速起步 ###

最近的项目都是用dojo来写，有一段时间没写angular代码了，貌似很多东西都忘记了，
主要是基础不牢固，现在angular2都已经出来了，果断投入angular2的怀抱，
趁着年底没项目做，前几天开始学习angular2，在这里写下学习angular2的笔记。

 **[示例代码](https://github.com/ytmjatai/angular2Demo)**

首先从 [中文官网](https://www.angular.cn/ 'angular2中文官方网站') 的
[开发指南-开发环境](https://www.angular.cn/docs/ts/latest/guide/setup.html) 一节
按照提示克隆《快速起步》并搭建本地开发环境，然后就可以开始愉快地玩爽了。

### ng-bootstrap ###
我们做项目一般会选择一个合适的UI库，这里首推
 [ng-bootstrap](https://ng-bootstrap.github.io/#/getting-started) 
这是一款基于Angular 2版本的Angular UI Bootstrap库。 
这个库是使用 Bootstrap 4 CSS框架从头开始编写的。

我们用npm把它当依赖安装到项目中

    npm install --save @ng-bootstrap/ng-bootstrap

同时安装 bootstrap4 CSS框架

    npm install --save bootstrap@4.0.0-alpha.6 

安装好后，在主模块中导入NgbModule模块，注意在imports数组中是 **NgbModule.forRoot()**

    import {NgbModule} from '@ng-bootstrap/ng-bootstrap';

    @NgModule({
    declarations: [AppComponent, ...],
    imports: [NgbModule.forRoot(), ...],
    bootstrap: [AppComponent]
    })
    
别忘记了在systemjs.config.js中加上下面一句，指示systemjs在何处找ng-bootstrap.js

    map: {
        '@ng-bootstrap/ng-bootstrap': 
        'node_modules/@ng-bootstrap/ng-bootstrap/bundles/ng-bootstrap.js',
    }

此时，如果你在项目中使用ng-bootstrap的组件，你会发现组件界面很乱，
那是因为我们还没有引入bootstrap 样式表。

在根目录的index.html中加上

    <link rel="stylesheet" 
          href="node_modules/bootstrap/dist/css/bootstrap.min.css">

好了，到这里我们就可以使用ng-bootstrap的组件了，组件详细用法请到
[官方网站](https://ng-bootstrap.github.io/#/components) 查阅相关文档



