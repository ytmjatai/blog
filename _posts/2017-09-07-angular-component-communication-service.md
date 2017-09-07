--- 
layout: article 
title: angular 组件通讯 - 服务
tags: angular
keywords: angular组件通讯,angular服务
description: angular组件通过服务通讯
---
# {{page.title}}

我们将通过一个例子学习 angular 组件间通过服务来通讯。如下图，左边的组件是个表单，可以输入
用户信息；右边的组件将通过服务来获取到左边组件的信息，并能处理这些信息

![示例图片](/assets/images/angular-component-communication1.png)

### service 服务组件 ###

我们要使用服务通讯，必须先建立一个 service 服务。
communicate.Service.ts

    import { Injectable, EventEmitter } from '@angular/core';
    @Injectable()
    export class CommunicateService {
        private _model: Model;
        get model(): Model {
            return this._model;
        }

        modelChange: EventEmitter<Model>;
        constructor() {
            this._model = new Model();
            this._model.id = null;
            this._model.name = null;
            this._model.age = null;
            this.modelChange = new EventEmitter<Model>(true);
        }

        fireModelChange() {
            this.modelChange.next(this.model);
        }

    }
    export class Model {
        id?: number;
        name?: string;
        age?: number;
    }

我们在服务中引入 EventEmitter ，以便服务模型改变的时候可以向其他组件发射出一个事件，其他
组件可以订阅这个事件，并作相应的处理

### left 组件 ###

我们在 left 组件中注入上边的服务组件，并把服务里的模型字段绑定到表单，当点击“确定”的时候，
触发服务模型变化

left.component.ts

    import { Component } from '@angular/core';
    import { CommunicateService } from './../../services/communicate.service';
    @Component({
        moduleId: module.id,
        selector: 'left',
        templateUrl: './left.component.html',
        styleUrls: ['./left.component.css']
    })
    export class LeftComponent {
        constructor(
            private comService: CommunicateService
        ) { }

        sub() {
            this.comService.fireModelChange();
        }
    }

left.component.html

    <div>
        <form>
            <div class="form-group">
                <label>Id</label>
                <input type="text" class="form-control" placeholder="Enter id" [(ngModel)]="comService.model.id" name="Id">
            </div>
            <div class="form-group">
                <label>Name</label>
                <input type="text" class="form-control" placeholder="Enter name" [(ngModel)]="comService.model.name" name="Name">
            </div>
            <div class="form-group">
                <label>Age</label>
                <input type="text" class="form-control" placeholder="Enter age" [(ngModel)]="comService.model.age" name="Age">
            </div>
            <button type="buttom" (click)="sub()" class="btn btn-primary">确定</button>
        </form>
    </div>

### right 组件 ###

我们在 right 组件引入 Subject 订阅服务模型的变化事件，在服务模型变化时作相应处理，在这里
我们让模型的 id 字段加上10，让 age 字段加上5，并在 right 组件模板显示出这个变化。最后，
不要忘记在组件销毁阶段取消订阅.

right.component.ts

    import { Component, OnInit, OnDestroy } from '@angular/core';
    import { Subject } from "rxjs/Subject";
    import { CommunicateService, Model } from './../../services/communicate.service';
    @Component({
        moduleId: module.id,
        selector: 'right',
        templateUrl: './right.component.html',
        styleUrls: ['./right.component.css']
    })

    export class RightComponent implements OnInit, OnDestroy {
        private comSubject: Subject<Model>;
        id: number;
        name: string;
        age: number;
        constructor(
            private comService: CommunicateService
        ) { }

        ngOnInit() {
            this.comSubject = this.comService.modelChange.subscribe(
                () => this.doSomething()
            )
        }

        ngOnDestroy() {
            this.comSubject.unsubscribe();
        }

        doSomething() {
            this.id = Number(this.comService.model.id) + 10;
            this.name = this.comService.model.name;
            this.age = Number(this.comService.model.age) + 5;
        }
    }

right.component.html

    <div>
        <p>{{name}} 您好！</p>
        <p>您的 Id 变成了:{{id}} </p>
        <p>我们让您长大5岁，您现在是：{{age}} 岁</p>
    </div>

 最终效果如下 **[示例代码](https://github.com/ytmjatai/component-communication-demo)** 

![最终效果图](/assets/images/angular-component-communication2.png)

