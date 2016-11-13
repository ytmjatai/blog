--- 
layout: article 
title: dojo-promise-all实现多个promise完成后,再返回一个新的promise
tags: Dojo
keywords: dojo/Deferred,dojo/promise/all
description: dojo-deferred,dojo-promise-all实现多个promise完成后,再返回一个新的promise
---
# {{page.title}}

前几天做项目的时候遇到这样的需求: 请求高德地图API的"路径规划"服务时返回的数据字段中含有火星坐标数组,这跟esri地图的坐标有偏差,所以要请求后台接口转换.

问题来了,因为这个火星坐标的数组长度是不固定的,所以要把每一个坐标对当作参数传给后台接口,以便计算出正确的esri地图坐标

***
__我把需求简单地模拟成这样:对一个数组的每一项执行一个异步操作,要求最后的返回结果按照原数组元素传递的顺序返回__ 

***

原来我是这样写的代码:

		require([
		    'dojo/Deferred'
		], function(Deferred) {

		    var arr = [1, 2, 3, 4, 5, 6, 7];

		    arr.forEach(function(item) {
		   		asyncProcess(item).then(
		            function(data) {
		                console.log(data);
		            }
		        )
		    })

		    function asyncProcess(number) {

		        var deferred = new Deferred();
		        var result = [];
		        var time = 1000 * Math.random();

		        setTimeout(function() {
		            result = number * 2;
		            deferred.resolve(result)
		        }, time)

		        return deferred.promise;
		    }

		})

我们执行代码会发现,由于每个asyncProcess执行的时间不确定,所以当数组执行完第3个元素时,第2个元素可能还没开始执行,所以这样无法确定最后结果是按数组元素传递顺序返回的.

这个时候,我们的dojo/promise/all就派上用场了.dojo-promise-all实现多个promise完成后,再返回一个新的promise.
简单用法如下:

		all(objectOrArray).then(
			function(results){
				//
			}
		)

我们改写上边的例子:

		<script type="text/javascript">
		require([
		    'dojo/Deferred',
		    'dojo/promise/all'
		], function(Deferred, All) {

		    var arr = [1, 2, 3, 4, 5, 6, 7];
		    var b = [];

		    arr.forEach(function(item, index, array) {

		        b[index] = asyncProcess(item)
		    })

		    All(b).then(
		        function(results) {
		            console.log(results)
		        }
		    )

		    console.log('异步操作，继续执行...')

		    function asyncProcess(number) {

		        var deferred = new Deferred();
		        var result = [];
		        var time = 1000 * Math.random();

		        setTimeout(function() {
		            result = number * 2;
		            deferred.resolve(result)
		        }, time)

		        return deferred.promise;
		    }

		}) 


[demo源代码](https://github.com/ytmjatai/dojo)







