--- 
layout: article 
title: 使用可观察对象 Subject 实现在 angular 中通讯
tags: angular rxjs subject
keywords: angular rxjs subject
description: 使用可观察对象 subject 实现在 angular 中传递参数
---

# {{page.title}}

### 可观察对象 Subject ###

Subject 是一种特殊的可观察对象，它允许将值多播给多个观察者。这意味着使用 Subject,我们可以在　angular 中实现一个组件发出消息，其他的多个组件能同时响应

使用可观察对象一般有 4 个核心步骤: 创建、订阅、执行、清理。

下面演示如何在 angular 中使用 Subject 实现组件传递参数。

### subject.service.ts ###
新建文件 subject.service.ts ，以便在其他组件中使用。

代码：
```js
import { Injectable } from '@angular/core';
import { Subject } from 'rxjs/internal/Subject';

@Injectable({
  providedIn: 'root'
})
export class SubjectService {

  private obj: { [key: string]: Subject<any> } = {};

  constructor() { }
  // 在组件用调用此方法，便会创建 Subject 并发送值 value
  push(keyName: string, value: any) {
    if (!this.obj[keyName]) {
      this.obj[keyName] = new Subject();
    }
    this.obj[keyName].next(value);

  }

  // 在组件中调用此方法，便能订阅其他组件创新的可观察对象 Subject
  pull(keyName: string) {
    if (!this.obj[keyName]) {
      this.obj[keyName] = new Subject();
    }
    return this.obj[keyName].asObservable();
  }
}

```
### a.component.ts ###
在 同一目录下新建 a.component.ts

代码:
```js
import { Component, OnInit } from '@angular/core';
import { SubjectService } from './subject.service';

@Component({
  selector: 'app-a',
  templateUrl: './a.component.html',
  styleUrls: ['./a.component.scss']
})
export class AComponent implements OnInit {

  constructor(
    private subjectSvc: SubjectService
  ) { }

  ngOnInit() { }

  // 触发此方法,传入任意类型值 value
  public setValue(value: any) {
    // 创建
    this.subjectSvc.push('numberValue', 3);
    // value = { name: 'jatai', age: 18 }
    this.subjectSvc.push('keyNameShuiBianXie', value); 
  }

}

```
### b.component.ts ###
在 同一目录下新建 b.component.ts

代码:
```js
import { Component, OnInit, OnDestroy } from '@angular/core';
import { SubjectService } from './subject.service';
import { Subscription } from 'rxjs';

@Component({
  selector: 'app-b',
  templateUrl: './b.component.html',
  styleUrls: ['./b.component.scss']
})
export class BComponent implements OnInit, OnDestroy {

  value$: Subscription;
  number$: Subscription;

  constructor(
    private sbjSvc: SubjectService
  ) { }

  ngOnInit() {
    // 订阅
    this.value$ = this.sbjSvc.pull('keyNameShuiBianXie').subscribe(
      // 执行
      value => console.log(value) // { name: 'jatai', age: 18 }
    );
    this.number$ = this.sbjSvc.pull('numberValue').subscribe(
      number => console.log(number) // 3
    );
  }

  ngOnDestroy() {
    // 清理 (取消订阅)
    this.value$.unsubscribe();
    this.number$.unsubscribe();
  }

}
```

上面演示了如何用可观察对象 Subject 在组件 AComponent 和组件 BComponent 中传递参数, 我们
还可以创建更多的组件来创建/订阅可观察对象。

同时我们发现，可以通过给 SubjectService 的方法 push(), pull() 传递不同的值，从而创建不同
的新的可观察对象。

最后在组件销毁时，清理可观察对象以免造成内存泄露。
