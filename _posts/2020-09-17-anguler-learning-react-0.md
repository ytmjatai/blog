--- 
layout: article 
title: Angular 菜鸡学 react 之配置项目
tags: react react
categories: react
keywords: react 
description: react 学习
intro: 项目接近尾声, 搬砖任务没那么重了, 腾出时间学点东西. 由于公司技术栈是 angular, 项目都是 angular-cli 脚手架, 对于前端构建那块早忘得差不多了, 今天就来重新学习. 这里用 react 来学习, vue 不会, 太难了, 等需要的时候再学
---

# {{page.title}}

----
##### 引言 #####
项目接近尾声, 搬砖任务没那么重了, 腾出时间学点东西. 由于公司技术栈是 angular, 项目都是 angular-cli 脚手架, 对于前端构建那块早忘得差不多了, 今天就来重新学习. 这里用 react 来学习, vue 不会, 太难了, 等需要的时候再学 
<a class="d-block" href="https://github.com/ytmjatai/react-demo" target="_blank">源代码</a>

----
##### 初始化项目 & Webpack 配置 #####
配置 入口/出口, 开在 index.html 引入打包后的 bundle.js, 可行.

<abc>webpack.config.dev.js</abc>
```js
const path = require('path');
module.exports = {
  mode: 'development',
  entry: './src/index.tsx',
  output: {
    path: path.resolve('./dist/'),
    filename: 'bundle.js'
  }
}
```

----
##### 使用 tsx 写 react #####
安装 react react-dom  babel  @babel/preset-react 等依赖, 并配置 babel. 也走得通.
<abc>.babelrc</abc>
```js
{
  "presets": [
      "@babel/env",
      "@babel/preset-react"
  ]
}
```
----

##### 使用 sass 写 css #####

安装 node-sass 和 sass-loader 等依赖, 并配置 webpack. 也没出错.
<abc>webpack.config.dev.js</abc>
```js
const path = require('path');
module.exports = {
mode: 'development',
  entry: './src/index.tsx',
  output: {
    path: path.resolve('./dist/'),
    filename: 'bundle.js'
  },
  resolve: { extensions: ['*', '.js', '.jsx', '.tx', '.tsx'] },
  module: {
    rules: [
      {
        test: /\.(js|ts)x?$/,
        exclude:  path.resolve('node_modules/'),
        include:  path.resolve('src/'),
        loader: 'babel-loader'
      },
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      },
      {
        test: /\.(sass|scss)$/,
        use: ['style-loader', 'css-loader', 'sass-loader']
      }
    ]
  }
}
```
----

##### webpack 其他配置 #####

配置 webpack-dev-server, HtmlWebpackPlugin, 并监听文件有修改时自动刷新浏览器
<abc>webpack.config.dev.js</abc>
```js
...
devServer: {
    port: 3000,
    open: true,
    hot: true,
    index: 'index.html'
},
watch: true,
watchOptions: {
    ignored: path.resolve('node_modules/'),
    aggregateTimeout: 500,
    poll: 1000
},
plugins: [
    new HtmlWebpackPlugin({
        title: 'React jatai',
        template: 'index.html'
    })
]
...
``` 
---

##### 添加 antd 和 bootstrap #####
安装完 bootstrap antd @types/react 等依赖后, 从 antd 官网弄个了 Layout 布局的栗子, 
发现在 vscode 上波浪线红了一片, 原来是语法不支持, 上网找了解决方法, 安装 @babel/plugin-proposal-class-properties 并配置,
最后再弄个按需加载 antd 的样式 

<abc>.babelrc</abc>
```js
{
  "presets": [
    "@babel/env",
    "@babel/preset-react"
  ],
  "plugins": [
    "@babel/plugin-proposal-class-properties",
    ["import", { "libraryName": "antd", "style": "css" }] 
  ]
}
```
---

##### 结语 #####
至此, 一个可以用 tsx 写 react 的项目基本结构就出来了. **[源代码](https://github.com/ytmjatai/react-demo)**

---

