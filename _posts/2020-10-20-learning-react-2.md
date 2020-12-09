--- 
layout: article 
title: React 学习笔记
tags: react react
categories: react 
keywords: react 
description: react 学习
intro: 上一篇文章写到 “用代码控制 react-router 导航的时候又有莫名其妙的坑” 就是在组件里不能通过调用 不能通过 this.props.history.push('/login')  返回到登陆界面, 这个可以用 withRouter解决
---

# {{page.title}}

----
#### withRouter ####
withRouter()  把不是通过路由切换过来的组件中, 将react-router 的 history、location、match 三个对象传入props对象上.
上次遇到在 Profile 里不能通过 this.props.history.push('/login') 退出登陆, 用把组件传难 withRouter() 再导出就可以了

----
#### class 组件 ####
生命周期函数 componentDidMount 里订阅/加载远程数据, componentWillUnmount 取消订阅与执行其他释放内在的操作, 
没啥好写的, 与 angular 组件里的生命周期都差不多, 换了名字而已

----
#### hooks ####

hook 可以在不编写 class 的情况下使用 state 以及其他的 React 特性, 不用写 this.state 那些冗余的代码, 这个挺不错的, 
用 hook 写了几个组件后, 在 react 里再也不想写 class 组件了.

useState 定义初始值与返回改变初始值函数的方法, 只在组件首次渲染的时候执行

useEffect 完成副作用操作, 给 useEffect() 传递第二个参数(数组), 当数组里的值改变时 useEffect() 执行(第一个函数参数), 
如果传入一个空数组, 则 useEffect() 只会执行一次.

useImperativeHandle 与 forwardRef 一起使用, 把子组件的方法暴露给父组件调用. 

<abc> edit.tsx </abc>
```tsx
import React, { forwardRef, useImperativeHandle, useEffect, useState } from 'react';
import { Form, TreeSelect, Input, InputNumber } from 'antd';
import * as cateSvc from '../../../services/category.service';
import { ICategory } from '../../../models/category';
import { ICommomTreeData } from '../../../models/antd-tree';

const Edit = forwardRef((props: PropsModel, eref) => {

  const cate = cateSvc.cateSelect$.getValue();
  const action = cateSvc.action$.getValue();
  const [treeData, setTreeData] = useState<ICommomTreeData[]>([]);
  const [form] = Form.useForm<ICategory>();
  switch (action) {
    case 'add':
      form.setFieldsValue({
        parentId: cate.id
      });
      break;
    case 'edit':
      form.setFieldsValue(cate);
      break;
    default:
      break;
  }

  useImperativeHandle(eref, () => ({
    onSubmit: async () => {
      const valid = await form.validateFields();
      if (!valid) { return false; }
      await doSubmit();
    }
  }));

  useEffect(() => {
    const action$ = cateSvc.action$.subscribe(action => onActionChange(action));
    return () => {
      action$.unsubscribe();
    }
  }, []);

  const onActionChange = (action: 'add' | 'edit') => {
    action === 'add' ? setTreeData(cateSvc.getTreeData()) :
      setTreeData(cateSvc.getDisableTreeData());
  }

  const doSubmit = async () => {
    const model: ICategory = form.getFieldsValue();
    switch (action) {
      case 'add':
        return await cateSvc.add(model);
      case 'edit':
        model.id = cate.id;
        if (!model.parentId) {
          model.parentId = null
        }
        const cateSelect = await cateSvc.edit(model);
        cateSvc.cateSelect$.next(cateSelect);
      default:
        break;
    }
  }

  return (
    <Form colon labelAlign="right" labelCol={{ span: 4 }} form={form}>
      <Form.Item label="分类名称" name="title" rules={[{ required: true }]}>
        <Input />
      </Form.Item>
      <Form.Item label="分类编码" name="code" rules={[{ required: true }]}>
        <Input />
      </Form.Item>
      <Form.Item label="上级分类" name="parentId">
        <TreeSelect treeData={treeData} allowClear />
      </Form.Item>
      <Form.Item label="排序" name="sequence">
        <InputNumber min={1} />
      </Form.Item>
    </Form>
  )
});

export default Edit;

interface PropsModel {
  onClose?: Function;
}
```

----

#### 结语 ####
以上是学习 react 的一些笔记, 在自己做的 图书管理系统 中用到. 
<a href="https://github.com/ytmjatai/react-demo" target="_blank" title='Django REST framework官网'>源代码</a> 

---