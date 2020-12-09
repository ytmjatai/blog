--- 
layout: article 
title: 实现一个 react 全局组件提示框
tags: react react
categories: react 
keywords: react 
description: react 学习
intro: 结合项目手摸手实现一个 react 全局提示框组件
---

# {{page.title}}
---

#### 前情 ####
在做 图书管理系统 项目书籍管理界面, 如果没有选中书籍分类, 那么在点击“删除”按钮的时候, 要弹出提示. 
因为项目里用的是 ant-design 组件库, 然而用的时候发现并没有找到我想要的弹出提示框. <br/>
Modal 用作提示框的话样式不好看, 舍弃. <br/>
Alert 没有自动关闭功能, 舍弃. <br/>
Notification 只有四大角位置, 我需要的是垂直和左右都居中, 舍弃.<br/>
Message 本来挺不错, 但没有自动关闭按钮, 也舍弃.<br/>
最后决定二次封装 Alert 用作全局提示组件.

---

#### 思路 ####
在其他组件里引入自定义的全局组件, 当点击“删除”按钮的时候, 调用一个函数, 传入参数, 弹出不同类型的提示框, 并且能根据传入的参数在适当的延时后自动关闭.

---

#### 阶段一 ####
先实现基础代码, 在其他组件调用 Toast.open() 能打开基本的提示框, 另外有css样式控制组件居中.

<abc>toast.tsx</abc>
```tsx
import React from 'react';
import ReactDOM from "react-dom";
import { Alert } from 'antd';
import './toast.scss';

const Toast = () => {
  return ( <Alert message="测试" /> );
}

Toast.open =  () =>{
  const hasDom = document.querySelector('body #global-toast');
  if (!hasDom) {
    const div = document.createElement('div');
    div.id = 'global-toast';
    document.body.appendChild(div);
    setTimeout(() => {
      ReactDOM.render(<Toast />, document.getElementById("global-toast"));
    });
  }
}

export { Toast };

```

<abc>toast.scss</abc>
```scss
#global-toast {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  min-width: 200px;
  z-index: 99999;
}
```

---

#### 阶段二 ####

把 Toast.open() 传进来的参数赋值到 Alert 里, 并且实现自动关闭及关闭后的处理.

通过在内部定义变量 visible 来控制显示/隐藏, 当 Alert 关闭后改变 visible 来隐藏.  
  
调用 Toast.open() 也要改变 visible 来达到显示效果, 否则 Alert 只会显示一次.

实现的关键点在于 Toast.open() 如何去改变 visible, 因为 setVisible() 才会触发 react 渲染, 而 open() 里不能调用 setVisible. 

我们可以借助定义一个可观察对象 open$, 在 open() 里将传进来的参数推送出去, 然后可以在 Toast 的 useEffect() 里监听参数的变化, 从而执行其他操作.

<abc>toast.tsx</abc>
```tsx

import React, { useState, useEffect } from 'react';
import ReactDOM from "react-dom";
import { Alert } from 'antd';
import { AlertProps } from 'antd/lib/alert';
import './toast.scss';
import { Subject } from 'rxjs';

const open$ = new Subject<Parameter>();

const Toast = (props) => {
  const p: Parameter = Object.assign({
    type: 'success',
    showIcon: true,
    closable: true
  }, { ...props.param })
  const [param, setParam] = useState(p);
  const [visible, setVisible] = useState(true);
  const onDestory = () => setVisible(false);

  useEffect(() => {
    const subscription$ = open$.subscribe((p) => onOpen(p));
    return () => {
      subscription$.unsubscribe();
    }
  }, []);

  const onOpen = (param: Parameter) => {
    setVisible(true);
    setParam(param);
  }

  return (
    visible ? <Alert
      message={param.message}
      type={param.type}
      showIcon={param.showIcon}
      closable={param.closable}
      afterClose={onDestory} /> : null
  );
}

Toast.open = (param: Parameter) => {
  open$.next(param);
  const hasDom = document.querySelector('body #global-toast');
  if (!hasDom) {
    const div = document.createElement('div');
    div.id = 'global-toast';
    document.body.appendChild(div);
    setTimeout(() => {
      ReactDOM.render(<Toast param={param} />, document.getElementById("global-toast"));
    });
  }
}

export { Toast };

interface Parameter {
  message: string;
  type?: AlertProps['type'];
  showIcon?: boolean;
  closable?: boolean;
}

```

---

#### 阶段三 ####

如果连续不断地按“删除”按钮, 那么就会不断地跳出提示框, 这还能改进. 
并且第一次打开并没有自动关闭, 这可以利用 useState() 只在组件首次渲染的特性, 传入一个一次性函数 setTimeout 执行关闭.

<abc>toast.tsx</abc>
```tsx
import React, { useState, useEffect } from 'react';
import ReactDOM from "react-dom";
import { Alert } from 'antd';
import { AlertProps } from 'antd/lib/alert';
import './toast.scss';
import { Subject } from 'rxjs';

const open$ = new Subject<Parameter>();
let isOpen = false;
const DURATION = 3000;
const Toast = (props) => {
  isOpen = true;
  const p: Parameter = Object.assign({
    type: 'success',
    showIcon: true,
    closable: true,
    duration: DURATION
  }, { ...props.param })
  const [param, setParam] = useState(p);
  const [visible, setVisible] = useState(true);
  const onDestory = () => setVisible(false);

  useState(() => {
    setTimeout(() => {
      setVisible(false);
      isOpen = false;
    }, p.duration);
  });

  useEffect(() => {
    const subscription$ = open$.subscribe((p) => onOpen(p));
    return () => {
      subscription$.unsubscribe();
    }
  }, []);
  
  const onOpen = (param: Parameter) => {
    if (isOpen) { 
      return; 
    }
    const p = Object.assign({
      type: 'success',
      showIcon: true,
      closable: true,
      duration: DURATION
    }, { ...param });
    setParam(p);
    setVisible(true);
    setTimeout(() => {
      setVisible(false);
      isOpen = false;
    }, p.duration);
  }

  return (
    visible ? <Alert
      message={param.message}
      type={param.type}
      showIcon={param.showIcon}
      closable={param.closable}
      afterClose={onDestory} /> : null
  );
}

Toast.open = (param: Parameter) => {
  open$.next(param);
  const hasDom = document.querySelector('body #global-toast');
  if (!hasDom) {
    const div = document.createElement('div');
    div.id = 'global-toast';
    document.body.appendChild(div);
    setTimeout(() => {
      ReactDOM.render(<Toast param={param} />, document.getElementById("global-toast"));
    });
  }
}

export { Toast };

interface Parameter {
  message: string;
  type?: AlertProps['type'];
  showIcon?: boolean;
  closable?: boolean;
  duration?: number;
}

```
---

#### 总结 ####
<a href="https://github.com/ytmjatai/react-demo" target="_blank" title='Django REST framework官网'>源代码</a> 
<br/>效果图:
<img class="d-flex w-100" src="/assets/images/react-component0.png" >

---







