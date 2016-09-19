---
layout: post
category : javascript
title: "解析angular模块的创建过程"
tags : [angular, module创建，javascript闭包]
---
{% include JB/setup %}


### 从立即调用函数把angular注册到window

>- 在执行setupModule函数中把window对象传入函数内部，通过ensure函数把angular注册到window对象上
>
>- 接着在注册一个module属性到angular对象上，属性值指向闭包函数module,
>
>- 当执行angular.module()方法时，把自定义的模块名注册在闭包环境中的modules={}上，同时返回绑定了一序列方法（factory,controller）的moduleInstance对象返回到外部

<pre>
(function(window) {

	function setupModule(window) {
		function ensure(obj, name, factory) {
			return obj[name] || (obj[name] = factory());
		}
		//把angular注册到window上
		var angular = ensure(window, 'angular', Object);
		//把module注册都angular上，并返回angular['module']方法
		return ensure(angular, 'module', function() {
			var modules = {};
			//angular.module()调用时执行的方法
			return function module(name, requires, configFn) {
				//执行这个方法后，返回的模块实例
				return ensure(modules, name, function() {
					//返回的模块实例
					var moduleInstance = {
						controller: function() {
							console.log("I am controller");
						},
						factory: function() {
							console.log("I am factory");
						},
						service: function() {
							console.log("I am service");
						}
					};
					return moduleInstance;
				});
			}
		});
	}

	setupModule(window);

})(window);
//创建一个模块mymodule,并把moduleInstance返回给mymodule变量
var mymodule = angular.module('mymodule', []);
//执行controller方法
mymodule.controller(); //I am controller
</pre>