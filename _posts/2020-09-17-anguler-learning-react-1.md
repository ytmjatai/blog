--- 
layout: article 
title: Angular 菜鸡学 react 之踩坑与经验
tags: react react
categories: react 
keywords: react 
description: react 学习
intro: 上一篇文章写了学习用 webpack 搭建 react(tsx) 项目基本结构的简单配置, 这一篇记录一下 angular 菜鸡在学习 react 中所遇到的坑及解决方法.
---

# {{page.title}}

----
#### 引言 ####
上一篇文章写了学习用 webpack 搭建 react(tsx) 项目基本结构的简单配置, 这一篇记录一下 angular 菜鸡在学习 react 中所遇到的坑及解决方法. 
<a class="d-block" href="https://github.com/ytmjatai/react-demo" target="_blank">源代码</a>

----
#### 坑0:  <span class="text-danger">Route 标签中间不能有空格</span> ####
导致 Module1 能加载出来而 Module2 不能, 纠结了好久.

<abc>webpack.config.dev.js</abc>
```js
  <Switch>
    <Route exact path="/module1" component={Module1}></Route>
    <Route path="/module2" component={Module2}> </Route>
  </Switch>
```

----
#### 坑1:  <span class="text-danger"> 进入 Module1 后再刷新浏览器,404</span> ####

其实从我们打开浏览器 http://localhost:3000(加载了所需资源, /index.html, /bundle.js 等) 到点击 module1 进入到  http://localhost:3000/module1 界面, 浏览器地址改变和网页视觉上的变化(刷新dom)都是由 js 来完成的, 并没有再次从服务器上加载资源.

当刷新浏览器时, 由于地址变成了 http://localhost:3000/module1, 而 /module1/index.html, /module1/bundle.js 等资源并不存在, 所以就出现了 404 错误

准确来说这个不能算坑, 是我们习惯了用脚手架来做项目, 把很多基础都丢了. 配经 webpack-dev-server, 加上 historyApiFallback: true 就可以解决 

<abc>webpack.config.dev.js</abc>
```js
...
devServer: {
  port: 3000,
  open: true,
  hot: true,
  historyApiFallback: true,
  index: 'index.html' 
}
...
```
----

#### 经验0  <span class="text-danger"> 按需加载 js 要在 index.html 加 base 标签的 href="/" </span> ####

由于打包出来的 js 有点大, 做了代码分割后又做了按需加载模块, 然后进入 http://localhost:3000/home/module1 后一片空白,
看代码提示找不到 http://localhost:3000/home/ 下的某个 js 文件.

根据上边的经验又是加载资源错误, 应该让它去服务器根目录下加载资源, 给 index 加上 base 标签并设置 href="/" 就好
<abc>index.html</abc>
```js
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <base href="/">
  <title>React Demo</title>
</head>
<body>
  <div id="root"></div>
</body>
</html>
``` 
----

#### 其他 ####
由于不会 redux, 状态提升又麻烦, 在非父子组件间通讯, 第一时间想到了 rxjs, 那就上呗.

用 rxjs 写了个通用并能传任意值的类, 然而不会在 react 里写依赖注入. 妹的, 那就把这个类实例化后再导出给其他文件直接引入使用吧,
这不知道 react 的最佳实践是怎样的, 没人指导 真・难, 慢慢摸索吧.


<abc>rx-event.service.ts</abc>
```js
import { Subject } from 'rxjs/internal/Subject';

class RxEventService {
  private events: { [key: string]: Subject<any>; } = {};
  private values: { [key: string]: any } = {};

  constructor() {  }

  on(eventName: string) {
    if (!this.events[eventName] || this.events[eventName].isStopped) {
      this.events[eventName] = new Subject<any>();
    }
    return this.events[eventName];
  }

  publish(eventName: string, val: any) {
    if (!this.events[eventName] || this.events[eventName].isStopped) {
      this.events[eventName] = new Subject<any>();
    }
    this.values[eventName] = val;
    this.events[eventName].next(val);
  }

}
const rxSvc = new RxEventService();
export default rxSvc;
``` 
---

#### 结语 ####
基本可以使用了, 然后用代码控制 react-router 导航的时候又有莫名其妙的坑, 等弄好了再写一篇学习笔记 . **[源代码](https://github.com/ytmjatai/react-demo)**

---