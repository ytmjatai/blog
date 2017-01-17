--- 
layout: article 
title: angular2学习笔记2-路由
tags: angular
keywords: angular2路由,ng-router
description: angular2路由学习笔记
---
# {{page.title}}

### 快速起步 ###

0.0 好吧，我一般是根据项目需求来学习东西的，angular在前端的用武之地是让搭建单页应用
变得更简单，而单页应用离不开路由导航，如果你只想写一个展示 "Hello World" 的单面应用，
那学习angular还有什么意思？我们从项目出发，学习angulr2的路由

 **[示例代码](https://github.com/ytmjatai/angular2Demo)**



### 添加组件并整理目录结构 ###
为了测试并使用路由，我们在项目中添加app.component.html并修改app.component.ts，
再添加项目所需组件，最后的目录结构应该如下图所示

![目录结构图](/assets/images/angular2-router1.png)

我们要实现点击不同的导航时，路由器能为我们加载不同的组件(即切换不同的页面)，效果如下

![主页效果图](/assets/images/angular2-router2.png)
![文档页效果图](/assets/images/angular2-router3.png)
![用户页效果图](/assets/images/angular2-router4.png)

### 导入并配置路由 ###
要使用路由功能，要先导入并配置，在主模块app.module.ts中导入路由和所需组件,
最后我们的文件应如下面的样子

    import { NgModule } from '@angular/core';
    import { BrowserModule } from '@angular/platform-browser';
    //导入路由
    import { RouterModule, Routes } from '@angular/router';
    // 导入所需组件
    import { NgbModule } from '@ng-bootstrap/ng-bootstrap';
    import { HeaderComponent } from './header/header.component';
    import { UsersComponent } from './users/users.component';
    import { UserEditComponent } from './users/user-edit.component';
    import { DocsComponent } from './docs/docs.component';
    import { MainComponent } from './main/main.component';
    // 定义路由配置
    const appRoutes: Routes = [
        { path: 'users', component: UsersComponent },
        { path: 'docs', component: DocsComponent },
        { path: '', component: MainComponent },
    ];
    //配置NgModule
    @NgModule({
        imports: [
            BrowserModule,
            NgbModule.forRoot(),
            RouterModule.forRoot(appRoutes)
        ],
        declarations: [
            AppComponent,
            HeaderComponent,
            UsersComponent,
            DocsComponent,
            MainComponent,
        ],
        bootstrap: [AppComponent]
    })

### 所需组件 ###
在主模块中导入并配置好路由后，我们简单介绍一下刚才导入的组件.
#### HeaderComponent ####
首先是 HeaderComponent

header.comoponent.ts代码如下:

    import { Component } from '@angular/core';

    @Component({
        // 使组件模板文件可以使用header.comoponent.ts文件的相对路径
        moduleId: module.id,
        // 
        selector: 'common-header',
        //模板文件
        templateUrl: 'header.html',
    })
    export class HeaderComponent {
        // navs为header.html模板文件所需数据
        navs = [
            { id: 1, title: 'Home', url: '/' },
            { id: 2, title: 'Users', url: 'users' },
            { id: 3, title: 'Docs', url: 'docs' }
        ];
    }

对应的模板文件header.html代码如下:

    <nav class="navbar navbar-toggleable-md navbar-inverse bg-inverse">
        <div class="collapse navbar-collapse" id="navbarNav">
            <div class="container">
                <ul class="navbar-nav">
                    <!--
                    RouterLinkActive 指令会基于当前的 RouterState 对象
                    来为激活的 RouterLink 添加 'class'
                    [routerLinkActiveOptions] 为精确匹配
                    -->
                    <li *ngFor="let nav of navs" 
                        routerLink="{{ nav.url }}" 
                        routerLinkActive="active"
                        [routerLinkActiveOptions]="{ exact: true }"
                        routerLink 
                        class="nav-item">
                        <a class="nav-link" href="{{ nav.url }}">{{ nav.title }}</a>
                    </li>

                </ul>
            </div>
        </div>
    </nav>

上面代码中的 *ngFor 作用相当于 angular1.x 中的 ng-repeat ,用于迭代并显示数据，
这里不详述.

#### MainComponent ####

main.component.ts代码如下:
    import { Component } from '@angular/core';

    @Component({
        moduleId: module.id,
        selector: 'main',
        templateUrl: 'main.html',
    })
    export class MainComponent {
        content='main page'
    }

对应的模板文件main.html的代码如图:

![main模板代码图](/assets/images/angular2-router5.png)

其他组件跟MainComponent类似

### 在页面中设置 ###
完成了以上步骤，我们在app.component.html页面添加以下代码

    <!-- HeaderComponent组件视图 -->
    <common-header></common-header>
    <!-- 路由插座，作用当于 ui-router 和 ng-router  --> 
    <router-outlet></router-outlet>

最后，还要在根目录下的index.html文件的<head>标签中添加

    <base href="/">

### 搬完收工 ###
至此，运行项目，点击不同的链接时，路由就会为我们导航到不同的页面



