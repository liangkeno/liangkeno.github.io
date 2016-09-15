---
layout: post
category : javascript
tagline: "Supporting tagline"
title: "函数的结构"
tags : [function, constructor, prototype]
---
{% include JB/setup %}

## 从函数的数据结构看原型 ##
#### 从一个函数声明入手 ####
>声明一个函数对象，并使用new实例化该对象，后输出

<pre>
function person() {
	this.sex = "girl";
}
person.prototype.name = 'prototypeName';
var instance = new person();
console.dir(instance);
</pre>
### 从构造函数实例化的普通对象 ###
*使用chrome 浏览器看到如下效果*
![](http://i.imgur.com/eUgr4HQ.jpg)
>从输出的结果可以看到，当前的对象为普通对象，定义了一个sex的属性，同时还自动生成一个内部属性\_\_proto\_\_,此属性为一个对象类型，指向构造函数person的prototype.
>
>\_\_proto\_\_为实例化后的对象内部属性（js内部使用寻找原型链的属性，只有ff与chrom可访问）
>
>prototype为函数对象的内部属性
##从普通对象中的\_\_proto\_\_指向的构造函数的原型##
*展开\_\_proto__后，可看到如下图：*
![](http://i.imgur.com/9Q2xkeF.jpg)

>从上图可以看出，person的原型包括三个属性，
>
> 1. 一个name是自定义的属性；
> 
> 2. constructor指向的是构造函数；
> 
> 3. \_\_proto\_\_指向一个通过