---
layout: post
category : javascript
tagline: "Supporting tagline"
title: "图解一个简单函数的结构关系"
tags : [function, constructor, prototype]
---
{% include JB/setup %}

## 从函数的数据结构看原型
#### 从一个函数声明入手 

>声明一个函数对象，并使用new实例化该对象，后输出

<pre>
function person() {
	this.sex = "girl";
}
person.prototype.name = 'prototypeName';
var instance = new person();
console.dir(instance);
</pre>
### 从构造函数实例化的普通对象

*使用chrome 浏览器看到如下效果*
![](http://i.imgur.com/eUgr4HQ.jpg)
>从输出的结果可以看到，当前的对象为普通对象，定义了一个sex的属性，同时还自动生成一个内部属性\_\_proto\_\_,此属性为一个对象类型，指向构造函数person的prototype.
>
>\_\_proto\_\_为实例化后的对象内部属性（js内部使用寻找原型链的属性，只有ff与chrom可访问）
>
>prototype为函数对象的内部属性

### 分解\_\_proto\_\_指向的person构造函数的原型

*展开\_\_proto__后，可看到如下图：*

![](http://i.imgur.com/9Q2xkeF.jpg)


>从上图可以看出，person的原型包括三个属性，
>
> 1. 一个name是自定义的person原型属性；
> 
> 2. constructor指向的是person构造函数；
> 
> 3. \_\_proto\_\_指向的是Object构造函数的原型

### 分解constructor 指向的person函数对象
![](http://i.imgur.com/k94C2m1.jpg)

>- 从上图可以看到，常用的初始化的arguments伪数组参数，定义的形参个数length，及函数名person，同时还是有自身的原型对象prototype，与实例化的instance的\_\_proto\_\_的指向相等；
>
>- 在函数对象中也有一个\_\_proto\_\_属性，此属性指向为函数构造器Function的原型，Function的prototype为函数对象，**展开以后得到以下图：**

![](http://i.imgur.com/1qt2ZVp.jpg)

>从上图我们看到经常使用到的一些方法如：apply,bing,call,这些方法到定义在Function.prototype之中，通过Function实例化的函数对象，自带一个\_\_proto\_\_属性指向该原型对象，所以通过函数对象可以访问到这些方法

### 分解 Function构造器对象
>Function 作为生成函数对象的构造器，同时也是自身生成自身的构造器，可以打印输出Function对象，**结构如下：**

![](http://i.imgur.com/FuYlP8f.jpg)

>从上图可以看出它的结果与person结构类似，不同的地方是Funtion.prototype的指向为函数对象person.prototype的执行为普通对象，这就可以间接的说明了Function可以构造自身，执行如下代码可知\_\_proto\_\_与Function.prototype相等

<pre>
Function.prototype.constructor === Function //true
Function.__proto__ === Function.prototype //true
</pre>

### 分解Object函数对象
>Object函数对象由Function构造器生成，同时使用Object可生成javascript所用到普通对象


![](http://i.imgur.com/sb8IYXm.jpg)

在上图可以看到常用到的一些静态方法如keys,create,defineProperty等，其中\_\_proto\_\_指向Function的原型；自身属性prototype，用于所有对象继承的原型

![](http://i.imgur.com/qgT5kS9.jpg)

<pre>
Object.__proto__ === Function.prototype // true
Object.prototype.__proto__  //null
</pre>


### 以一张图概括函数的结构关系

<img src="http://i.imgur.com/nCI2gSS.gif" width="100%">
<pre>
//红色链条
instance.__proto__ === person.prototype;
person.prototype.__proto__ === Object.prototype;

//紫色链条
Object.prototype.__proto__ === null;

//蓝色链条
person.__proto__ === Function.prototype;
Function.__proto__ === Function.prototype;
Function.prototype.__proto__ === Object.prototype;

//黄色链条
Object.__proto__ === Function.prototype;
</pre>
