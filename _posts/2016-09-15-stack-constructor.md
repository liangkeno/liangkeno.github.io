---
layout: post
category : javascript
tagline: "Supporting tagline"
title: "js 实现栈的结构"
tags : [stack, data structure, prototype]
---
{% include JB/setup %}

## js实现一个栈的数据结构

>首先了解一下什么是栈，栈是一个后进先出的一种数据结构，执行起来效率比较高。
>
>对于栈主要包括一些方法，弹出栈pop(),弹出栈顶元素，并删除该元素；压入栈push(),向栈中压入某个方法，栈中的长度加一；读取栈顶元素peek(),仅读取不删除
>
>使用js的构造模式创建栈类，原型进行共享主要方法


### 代码实现如下

<pre>
(function(root) {
	function Stack() {
		this.dataStore = [];
		//数组的元素个数
		this.top = 0;
	}

	Stack.prototype = {
		pop: function() {
			//出栈时，主要使用前减运算，返回栈顶元素，元素个数减一
			return this.dataStore[--this.top];
		},
		push: function(elem) {
			//入栈时，使用后加运算符，先在栈顶添加元素，元素个数加一
			this.dataStore[this.top++] = elem;
		},
		peek: function() {
			return this.dataStore[this.top - 1];
		},
		clear: function() {
			//当清空栈时，访问栈顶的结果为undefined
			this.top = 0;
		},
		length: function() {
			return this.top;
		}
	}

	root.Stack = Stack;

})(global);


var stack = new Stack();
stack.push("liang0");
stack.push("liang1");
stack.push("liang2");
console.log(stack.peek());
console.log(stack.pop());
console.log(stack.peek());
stack.push("liang4");
console.log(stack.peek());
stack.clear();
console.log(stack.peek());
</pre>

### 执行结果：
![](http://i.imgur.com/VMkVpou.jpg)