--- 
layout: article 
title: angular 自定义管道
tags: angular
categories: angular
keywords: angular管道 angular自定义管道
description: angular自定义管理
intro: angular 有内置的管道，如 DatePipe ,在实际开发中，我们还可以自定义管道以满足项目需求。自定义管道非常简单，我们定义一个管道，它将根据传入参数来决定如何展示传入的数据
---
# {{page.title}}

angular 有内置的管道，如 DatePipe ,在实际开发中，我们还可以自定义管道以满足项目需求。
自定义管道非常简单，我们定义一个管道，它将根据传入参数来决定如何展示传入的数据，如果参数中的 "type"为"数字"类型，则取两位小数，如果 "type"为"文字"类型，刚原样输出。 如下代码：

    import { Pipe, PipeTransform } from '@angular/core';

    @Pipe({ name: 'fieldFormat' })

    export class FieldFormatPipe implements PipeTransform {

        public transform(value: string, field: FieldInfo): string {
            if (field.type === 'decimal' && field.dec) {
                return Number(value).toFixed(field.dec);
            }
            if (field.type === 'text') {
                return value;
            }
        }

    }

    export interface FieldInfo {
        name: string;
        type: string;
        dec?: number;
    }

首先在管道元数据中给管道起名字，这个名字是在模板里边的用的，和内置管道用法一样。然后导出自定义管道 FieldFormatPipe ,要在模板中使用这个管道，必须在 AppModule 的 declarations 中包含这个管道。

自定义的管道要实现 PipeTransform 接口的 transform 方法，该方法可以接收多个参数，第一个参数为要过滤的值，后面的为可选参数，可以传入任意值（一般有多个值的话建议组装成对象形式）

我们可以在模板中这样使用这个自定义的管道

    <tbody>
        <tr>
            <td *ngFor="let field of fields">
                <div> { { field.name | fieldFormat:field }}</div>
            </td>
        </tr>
    </tbody>

呃，就是这么简单，没了