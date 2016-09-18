---
layout: post
category : javascript
tagline: "Supporting tagline"
title: "测试一个requirejs的加载"
tags : [AMD,requirejs]
---
{% include JB/setup %}

##了解一下requirejs的使用##
requirejs属性AMD规范，AMD即异步模块定义，它采用异步方式加载模块，不影响它后面的语句的执行。

**优势**
> 1.异步加载模块，不会阻碍页面的渲染
> 
> 2.可保证加载顺序，可管理模块间的依赖，方便维护
> 
> 3.可进行多人协同工作

**必要的三个函数**
>define() 定义模块，响应require的调度
>
>require.config() 设置模块与路径的映射
>
>require() 程序的入口，包括两个参数，第一个为数组，表示依赖的模块；第二个参数为回调函数，它在当前页面指定的模块被加载完成才调用，加载的模块会以参数的形式传入该函数。

**demo 文件结构**

<pre>
requirejsDemo
	--index.html
	--js
	   --clouse-2.js
	   --main.js	
	   --requirejs.config.js
	--require.js
</pre>

**调试工具查看文件加载顺序**

![](http://i.imgur.com/h4lb8ex.png)

**mian.js文件为入口文件**

<pre>
//加载配置文件
require(["js/requirejs.config"], function() {
	//加载clouse_2模块
	require(["clouse_2"], function(clouse_2) {
		var btnModel = {
			name: "btnModel",
			click: function() {
				alert(this.name);
			}
		};
		var btn = document.getElementById('btn');
		//调用clouse_2.bind方法
		btn.addEventListener('click', clouse_2.bind(btnModel, "click"), false);
	});

});
</pre>

Please take a look at [{{ site.categories.api.first.title }}]({{ BASE_PATH }}{{ site.categories.api.first.url }})
or jump right into [Usage]({{ BASE_PATH }}{{ site.categories.usage.first.url }}) if you'd like.
