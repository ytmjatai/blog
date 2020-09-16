--- 
layout: article 
title: 使用 nodejs、express 和 mongoDB 实现 CURD
tags: nodejs express mongoDB
categories: other
keywords: nodejs express mongoDB
description: 使用 nodejs、express 和 mongoDB 实现简单后台系统的增删改查示例
intro: 使用 nodejs、express 和 mongoDB 实现简单后台系统的增删改查示例
---

# {{page.title}}

### 安装 ###

根据自己的系统安装 **[nodejs](https://github.com/nodesource/distributions/blob/master/README.md#debinstall)**、
    **[mongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-debian/)**、
    **[express](https://expressjs.com/en/starter/installing.html)**、
    **[express-generator](https://expressjs.com/en/starter/generator.html)**

### 生成 express 项目并安装依赖 ###
```bash
$ express library-manage-system

$ npm install mongodb --save

$ npm install

```
### 在 routes 目录下新建 books.js ###
```js
var mongodb = require('mongodb');
var express = require('express');
var router = express.Router();

router.get('/', function (req, res, next) {
    mongodb.MongoClient.connect(
        'mongodb://localhost',
        { useNewUrlParser: true },
        async (err, client) => {
            if (err) {
                console.error(err);
                return;
            }
            /*
             * library-manage-system 为 mongo 数据库
             * book 是数据库里边的集合（表）
             * 必须先在本地数据库里创建好并添加一些文档作测试, 不然没有数据显示
             */
            const db = await client.db('library-manage-system')
                .collection('book')
                .find({})
                .toArray();
            client.close();
            res.send(db);
        }
    );
});
module.exports = router;
```
### 引入 books.js 并配置路由 ###
参考项目默认生成的两个路由，在对应的地方相似地写上
```js
var booksRouter = require('./routes/books');
```
```js
app.use('/api/books', booksRouter);
```
### 运行项目 ###
```bash
$ npm start
```
此时打开 http://127.0.0.1:3000/api/books ,可以看到 library-manage-system 中 book
表里的数据

### 完善 books.js ###

跟着 **[Nodejs MongoDB Driver API](http://mongodb.github.io/node-mongodb-native/3.2/api/Collection.html)**、 文档，完成增删改查    

books.js
```js
var mongodb = require('mongodb');
var express = require('express');
const { ObjectId } = require('mongodb');

var router = express.Router();

router.get('/', function (req, res, next) {
    mongodb.MongoClient.connect(
        'mongodb://localhost',
        { useNewUrlParser: true },
        async (err, client) => {
            if (err) {
                console.error(err);
                return;
            }
            const db = await client.db('library-manage-system')
                .collection('book')
                .find({})
                .toArray();
            client.close();
            res.send(db);
        }
    );
});

router.get('/:id', function (req, res, next) {
    mongodb.MongoClient.connect(
        'mongodb://localhost',
        { useNewUrlParser: true },
        async (err, client) => {
            const id = req.params.id;
            if (err) {
                console.error(err);
                return;
            }
            const db = await client.db('library-manage-system')
                .collection('book')
                .find({ _id: ObjectId(id) })
                .toArray();
            client.close();
            res.send(db[0]);
        }
    );
});

router.post('/', function (req, res, next) {
    mongodb.MongoClient.connect(
        'mongodb://localhost',
        { useNewUrlParser: true },
        async (err, client) => {
            const book = req.body;
            if (err) {
                console.error(err);
                return;
            }
            const db = client.db('library-manage-system')
                .collection('book')
                .insertOne(book)
                .toArray();
            client.close();
            res.send(db);
        }
    );
});

router.delete('/', function (req, res, next) {
    mongodb.MongoClient.connect(
        'mongodb://localhost',
        { useNewUrlParser: true },
        async (err, client) => {
            const id = req.query.id;
            if (err) {
                console.error(err);
                return;
            }
            const db = client.db('library-manage-system')
                .collection('book')
                .deleteOne({ "_id": ObjectId(id) });
            client.close();
            res.send(db);
        }
    );
});

router.put('/:id', function (req, res, next) {
    mongodb.MongoClient.connect(
        'mongodb://localhost',
        { useNewUrlParser: true },
        async (err, client) => {
            const id = req.params.id;
            const book = req.body;
            if (err) {
                console.error(err);
                return;
            }
            const db = client.db('library-manage-system')
                .collection('book')
                .updateOne(
                    { "_id": ObjectId(id) },
                    {
                        $set: {
                            name: book.name,
                            author: book.author,
                            desc: book.desc
                        }
                    }
                );
            client.close();
            res.send('success');
        }
    );
});
module.exports = router;
```

### 查看效果 ###
至此，可以用 **[Postman](https://www.getpostman.com/)** 查看所有效果，也可以建立客户端界面查看。
我把客户端和后台整合到一个项目里，客户端用的是 **[Angular](https://angular.cn/)** ，涉及到跨域问题在代码里已解决.

**[源代码](https://github.com/ytmjatai/library-manage-system)**
