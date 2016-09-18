---
layout: post
category : javascript
tagline: "Supporting tagline"
title: "自我总结javascript的知识点之一"
tags : [javascript知识点, 总结]
---
{% include JB/setup %}

自我总结javascript的知识点之一
====================

### 数组去重

>- 把数组去重函数写到内置数组对象的原型，方便调用
>
>- 理解难点，this对象，新建空对象用于判断数组是否重复，this[i]作为空对象的键访问对象的值
>
>- 思路：使用this遍历数组各个元素，使用isExist[this[i]]判断当前元素是否存在，如果不存在就使用push添加到新建的返回数组中，同时把数组元素作为isExist对象的键值并赋值为1
<pre>
<code>
Array.prototype.unique = function() {
	var len = this.length,
		isExist = {},
		resultArr = [],
		i;
	for (i = 0; i < len; i++) {
		if (!isExist[this[i]]) {
			resultArr.push(this[i]);
			isExist[this[i]] = 1
		}
		//console.log(this[i]);
	}
	console.log(isExist);
	return resultArr;
}
var arr = [2, 8, 2, 9];
console.log(arr.unique()); 
</code>
</pre>

**执行结果**

![](http://i.imgur.com/sNRm1sH.png)
### 小心window对象的坑

>- window对象，全局变量，出错的this
>
>- 在函数最外层声明的变量及最外层声明的函数，这些都属于全局的对象之中
>
>- 没有使用var声明的变量,相当就是在window下添加的属性，使用delete可删除，不会出错

<pre>
 var getId=document.getElementById;
 var dom=getId("div");
</pre>

_上段代码执行结果_

![](http://i.imgur.com/OgQ13fQ.png)


>*解析* 新建一个变量getId即可获取getElementById的函数,但是传入参数执行的时候，由于声明的getId为全局变量，this指向了window,而非document，所以执行时会出错

**可使用下面的代码进行验证**
<pre>
 var getId=document.getElementById;
 var dom= getId.call(document,'div');
</pre>
*执行结果*
>无错误出现，主要是使用call改变了getId的this对象，使之由window变回为document


**当执行foo.fn()时，请问this指向那个对象**
<pre>
function foo(){};
foo.fn=function(){
	var bar=function(){};
	bar();
};
</pre>
*执行结果* 在foo函数对象上定义静态方法fn,函数里面的this应该指向当前的函数对象，但是在直接声明私有方法bar后，直接执行，由于bar()前面没有对象调用，所以默认为this默认为window
### 关于简单数据类型的转换

>- 简单数据类型主要包括Undefined,Null,Boolean,Number,String
>- Undefined类型只有一个值undefined，在var声明且未赋值时的默认值，如果变量未声明而使用就出现错误(not defined),但是使用 typeof 操作此变量的结果为undefined;
>- Null类型也是只有一个值null,常用用于表示空对象的指针，使用typeof操作时返回的是object,可设置变量的初始值为null用于未来保存对象或者清空某对象方便浏览器垃圾回收，由于undefined派生自null的值，所以undefined==null 为true
>- Boolean类型中的值为true 或者 false,在使用if或者Boolean()等...可进行转换，转化值为false的是：""(空字符串)，0和NaN(数字类型)，null(object类型)，undefined(Undefined类型)
>- Number类型使用比较多为十进制，另外还有八进制(以0开头序列为0-7)，十六进制(以0x开头，序列为0~9和A~F);+0===-0为true;NaN为非数字，但不等于自身；转换函数包括Number(),parseInt,parseFloat();
>- String类型，字符串一旦创建就不能改变只能销毁在创建，所以低版本的浏览器拼接字符串时会比较缓慢；转换字符串可使用toString()或者Stirng()方法

<pre>
	var value="100"+2-"1";
	console.log(value);
	console.log(typeof value);
</pre>
*执行结果*

![](http://i.imgur.com/G8nxaWp.png)

>*解析*：按照赋值运算符由右至左（算出右边的值赋值给左边的变量），赋值等号右边的运算顺序为左至右；所以遇到字符"100"时，优先+号的拼接操作，把数字2转为字符串即得1002，如遇到减号运算则把字符串转为数字，进行相减运算。

>每当读取一个Number,Boolean,String类型的值时，后台自动创建一个对应的类型实例，从而可调用对应的方法,当调用完方法以后就实例立即销毁
>
>如以string类型为例：

<pre>
	//例子1
	var backStr="hello world".substring(2)
	console.log(backStr);
	
	//解析后台自动创建，包含三个步骤
	//例子2
	var newStrObj=new String("hello world");
	var backStr=newStrObj.sunstring(2);
	newStrObj=null;
</pre>
### 比较运算符的转换
>比较等于或者不等于时，ECMAscript规定“**先转换在比较**”；比较全等于或者全不等于时，ECMAscript规定，“**仅比较不转换**”。
>
>*在相等或不等中遵循以下转换规则*
>
>- 如果有一个操作数为布尔值，在比较之前先把false转换为0，true转换为1，在进行比较；
>- 如果有一个操作数为数字，另一个为字符串，先把字符串转换位数字，在进行比较；
>- 如果有一个操作数为对象，另一个不是，则调用对象的valueOf方法转换为基本数据类型，在使用前面两个规则进行比较；
>- 如果两个操作数都是对象，则比较两个对象是否为同一个对象；
>- 比较时有几个特例 null == undefined 为true;NaN与任何数进行相等比较都返回false，与不等返回true;

<pre>
console.log((true == "true"));//false
console.log((2 == true));//false
console.log((1 === true ));//false
</pre>
*执行解析*：在执行相等操作时，布尔值被转换为数字，ture变为1，1与字符串"true"比较为false;2==1结果为false;在进行全等比较时，不转换，数值与布尔为不同类型，所以返回false.


### 解析一段javascript的执行过程
<pre>
var x = "global value";

function getValue(y) {
	alert(y);
	var x = "local value";
	alert(x);
}
getValue(2);
</pre>

>- 在运行一段js代码时，其中涉及的知识点比较多，比如进入页面时会自动创建全局的变量对象，生成全局作用域，设置this的指向为window对象，在这一过程就形成了全局的执行上下文，并压入执行上下文栈；
 - 构想的全局上下文，便于理解：
   <pre>
//全局执行上下文创建阶段
var globalExecutionContext = {
		scopeChain: {},
		variableObject: {
			x: undefined,
			getValue: function() {
				alert(x);
				var x = "local value";
				alert(x);
			}
		},
		this: window
};
//全局执行上下文执行阶段
var globalExecutionContext = {
			scopeChain: {},
			variableObject: {
				x: "global value",
				getValue: function() {
					alert(x);
					var x = "local value";
					alert(x);
				}
			},
			this: window
}
</pre>
>
>- 如果调用全局变量对象中的函数时，也会生成当前函数的执行上下文，包括生成函数的作用域；设置this的指向，可能指向window或者某一个对象；生成当前函数的激活对象，并以激活对象作为当前函数的变量对象。同时把当前函数的执行上下文压入执行上下文栈。
 - 构想的函数上下文，便于理解：
<pre>
//如果调用getValue函数时,创建函数执行上下文
var getValueFunctionExecutionContext = {
		scopeChain: {},
		activeObject: {
			arguments: {
				0: 23
				length: 0,
				callee: function() {
					alert(x);
					var x = "local value";
					alert(x);
				},
				propertiex - indexes: "unkonw",
				caller: null
			},
			y: 23,
			x: undefined
		}
		this: window
};
//执行getValue函数阶段的执行上下文
var getValueFunctionExecutionContext = {
	scopeChain: {},
	activeObject: {
		arguments: {
			0: 23,
			length: 0,
			callee: function() {
				alert(x);
				var x = "local value";
				alert(x);
			},
			propertiex - indexes: "unkonw",
			caller: null
		},
		y: 23,
		x: "local value"
	}
	this: window
}
</pre>
>
>- 在生成函数的变量对象时，分为两个阶段：创建阶段与激活阶段
 * 创建阶段：函数的变量对象是在进入函数的执行上下文时创建，js解释器主要做下面事情：
	   1. 初始化函数的参数对象。
	   2. 扫描函数内部的函数声明，把函数名指向对应的函数对象，并存入当前变量对象；如果当前函数变量对象的属性名与函数名相同，则使用函数名对应的函数对象进行覆盖。
	   3. 扫描函数内部的变量声明，把变量名设置为undefined，并存入当前变量对象；如果变量名与当前变量对象属性相同，则什么也不做。
	   <pre>
		var x = "global value";				
		function getValue(x) {
			alert(x);
			var x = "local value";
			alert(x);
		}
		getValue(23);
		//结果：23与 ”local value“
		</pre>
	首先，是在创建函数变量时，先初始化arguments对象，因为arguments是一种伪数组的数据结构，所以arguments[0]=23,实际参数个数为length=1，同时在变量对象添加同名参数属性x=23；接着扫描函数内部，发现var 声明的x变量，由于之前参数初始化时已经存在x变量，所以不做任何操作。
 * 激活阶段：即函数的执行阶段，当函数碰到第一个alert(x)时，会去当前函数的变量对象中查找x,此时x的值为23；接着对x进行赋值，碰到第二个alert(x),此时的x值为”local value“；如果第二个alert的末尾添加console.log("arguments[0]"),此时输出的值为：”local value“（由于变量对象与arguments采用实时映射的方式，当变量对象中的x值变化时，arguments形参的x的值也跟着变化）。

### 我理解的作用域链
>在执行一个函数或者进入一个页面时都会创建一个执行环境，而每个执行环境都关联对应的变量对象，这个变量对象定义当前的环境能访问的数据或者函数。
>
>- 在进入页面时创建对应全局变量对象，也称为全局作用域，定义的数据可被内部的所有函数局部变量对象访问；全局执行环境被压入执行栈的最底端，在应用程序退出或者页面被关闭时才被销毁。
>- 在执行一个函数时，会创建对应的局部变量对象，也叫激活对象，被称为局部作用域；激活对象定义了当前函数可访问的变量或者函数；局部变量对象作为函数执行上下文的一部分内容被压入执行栈中，当函数执行完毕后，局部变量对象自动被销毁。
>- 每个函数，当被执行的时候，变量对象被激活，即被创建，变成激活对象；添加到激活对象的数据只包括var 或者function声明的变量；每个激活对象都有一个内部属性\_\_parent\_\_指向父变量对象，全局变量的内部属性\_\_parent\_\_指向null.
>- 作用域链是由每个的变量对象或者激活对象关联起来的，在标识符的查找过程，只能由子变量对象往全局变量对象方向查找，方向不可逆。


**可通过一下例子说明**
<pre>
var a=10;
function foo(){
	var b=20;
	function bar(){
		var c=30;
		console.log(a+b+c);
	}
	bar();
	
}
foo();//输出结果：60
</pre>
_结果解析_：在进入这段代码时，会把变量a,foo添加到全局变量对象上；当执行foo()时，会把b,bar添加到foo函数的变量对象上；当执行到bar()时，也会添加c到bar函数的变量对象上；当执行输出a+b+c时，会对表示符a进行查找，首先在bar本身的变量对象进行查处，没找到，往父层的变量对象方向查找，直到在全局变量对象中找到a=10;b,c查找的方式也类似，最终输出60。
### 通过作用域链看闭包
>从形式上看，闭包是函数嵌套函数，同时把嵌套的函数返回到外部，在外部执行返回的嵌套的函数可以访问内部的定义的数据。从本质上看，由于是嵌套在里面的函数，本身就能访问外层函数的变量对象，属于构建的一条作用域链上。按照函数正常的执行，当外层的函数执行完毕时，构建的变量对象应该会被销毁，但是由于存在return 内部的函数，此时关于内部的函数作用域链上的变量对象并没有被销毁，所以返回嵌套的函数在外层函数之外执行仍然可以访问外层函数内部定义的数据。

<pre>
var a=10;
function foo(){
	var b=20;
	function bar(){
		var c=30;
		console.log(a+b+c);
	}
	return bar;
	
}
var fn=foo();
fn()//输出结果：60
</pre>

_结果解析_ ：在执行foo()完毕之后，会返回闭包bar函数，并赋值给fn。如果没有return 函数，foo()在执行完毕时，对应的变量对象的属性b,bar也会跟着销毁，但是当return 闭包函数bar时，foo变量对象还是存在内存中，当执行fn()时，即可访问闭包函数声明的环境中的作用域链条。所以输出结果为：60.

### 聊一聊new的工作方式
>在对一个构造函数进行new运算时，会经历一下步骤：
>
>1. 新建一个空对象；
>2. 将构造函数的作用域赋给这个空对象，同时this指向空对象
>3. 执行构造函数里面的代码，即为空对象添加属性
>4. 返回空对象
>
>在给这个空对象赋予构造函数时，即是把构造函数内部创建的变量对象赋给予空对象，它可以访问构造函数内部通过var 和function 声明的数据及此构造函数作用域链上的变量对象。在执行构造函数里面的代码时，可以为新对象添加属性，或者当前构造函数的私密变量赋值等操作。


**以下通过伪代码模拟new的工作方式**
<pre>
function newCreate(fn, arg) {
	var obj = {};
	obj.__proto__ = fn.prototype;
	fn.apply(obj, arg);
	return obj;
}

//新建一个构造函数
function superType() {
	var privateVlaue = "name";
	this.sex = "man";
	this.sayInfo = function() {
		console.log(privateVlaue + "::" + this.sex);
	};
}
superType.prototype.saySex = function() {
	console.log(this.sex);
};
var instance=newCreate(superType);
instance.saySex();//输出结果：man
</pre>

		   



