--- 
layout: article 
title: javascript Array类型
tags: javascript
keywords: array,array数组
description: 是时候温故知新javascript的array类型了(呃，我都是按自己平时使用array的方法及习惯来写的，比如新建一个数组，我不用new Array()这种形式，所以文章中难免会有很多写得不全的地方)。
---
# {{page.title}}

首先javascript中的Array类型可以保存任意类型数据，每个元素的结构不要求相同，并且可以动态地调整Array实例的长度

### array转string方法 ###

一般将数组转换为字符串用join()方法，给该方法传入一个分隔符，用来连接数组元素，可以传入空

	var arr = ['jatai','Peter','Jim'];
	arr.join('-')	// jatai-Peter-Jim

### array的增删方法 ###

arr.push() 向数组末尾添加 n 项,返回修改后的数组长度

arr.pop() 在数组末尾删除一项，返回该项

arr.shift() 在数组开关删除一项，返回该项

arr.unshift() 在数组末尾添加 n 项，返回修改后数组的长度



***
一般项目里用得最多的应该是push()了

***

### array的迭代方法 ###

arr.forEach() 对数组每一项进行指定操作

	arr.forEach(function(item,index,array){
		/*
		 *想做什么坏事可以在这里做，
		 *比如 当item是数值时，让它增加2倍再存入另一个数组
		 */
		item = item * 2;
		tempArr.push(item)
	})

arr.filter() 对数组每一项进行指定操作，返回结果为true组成的数组，一般用于筛选数组

arr.every() 判断数组每个元素是否都符合指定操作

arr.some() 判断数组是否有元素符合指定操作

arr.map() 这个你就当是有返回值的forEach()吧，它最后得到的结果是所有返回值组成的数组

	var tempArr = arr.forEach(function(item, index, array) {
        /*
         *这里用map()方法的话，
         *就不用再每个元素都存入数组了
         *它最后得到的是各个元素 *2 组成的数组
         */
        return item = item * 2;
    })

***
一般项目里用得最多的是forEcah()，掌握了forEcah(),其他几个就容易理解与运用了 

***

### 其他方法 ###

sort()排序，一般传入一个比较函数

	arr.sort(function(val1, val2) {
        if (val1 < val2) {
            return -1;
        } else(val1 > val2) {
            return 1;
        } else {
            return 0;
        }
    }) //升序排列数组

如果想降序排列，就把val1,val2的比较结果反过来，或者得到升序后，调用下面的reverse()方法

reverse() 把数组反转

***
有童鞋在这里会不会想到求数组的最大最小值方法？以前学到这两个方法时我的第一反应就是求数组最大最小值再也不用遍历了！后来发现有些不妥当，因为这两个方法会改变原数组！最后的扩展中我们将学习一种求数组最大最小值的简洁方法.

***

reduce() 把返回值作为参数（'prev'）传给下一次调用，看代码吧

	var arr = [1, 2, 3, 4, 5, 6, 7, 8];
    var reduce = arr.reduce(function(prev, cur, index, array) {
        console.log('上次的值：'+prev)
        console.log('当前的值：'+cur)
        console.log('当前索引：'+index)
        console.log(''+array)
        return prev + cur;//(prev+cur)作为下一次的 prev传入
    },9)
    console.log(reduce) //45
    /*
     *如果传入参数'9',则第一次时prev=9,cur=1,index=0
     *如果不传入参数,则第一次时prev=1,cur=2,index=1
     */

reduceRight() 跟reduce()一样，只是调用的顺序相反，也相当于把数组反转后调用reduce() 

网上有教程说reduce()这货的执行速度是for循环语句的10倍左右，也不懂真假，反正项目中能用的地方尽量用就是了

*** 

### __扩展__###






