---
layout: post
category : tools
title: "原生js写一个测试套件"
tags : [测试, assert]
---
{% include JB/setup %}

>- 测试套件中，使用的主要是断言函数，传入布尔值与描述，通过新建一个li描述，在测试通过时打上红色文字，否则绿色文字；
>- 使用test函数在视觉上封装一个测试组，主要时通过闭包的result变量实现；第一次将套件的标题插入在外围的结果标签中，返回的结果改变的result变量，执行回调函数陆续将套件的中的单元测试添加到result变量中。

**页面样式**

<pre><code>
&lt;style type=&quot;text/css&quot;&gt;
 .fail{color: red;}
 .pass{color: green;}
&lt;/style&gt;
&lt;ul id=&quot;result&quot;&gt;&lt;/ul&gt;

</code></pre>

**javascript实现**

<pre><code>
(function(){
	//闭包的结果变量
	var result;
	//暴露在全局的断言函数
	this.assert=function(value,desc){
		var li=document.createElement('li');
		value==true?li.className='pass':li.className='fail';
		var textNode=document.createTextNode(desc);
		li.appendChild(textNode);
		result.appendChild(li);
		if(!value){
			li.parentNode.parentNode.className='fail';
		}
		return li;
	}
	//暴露在全局的套件函数
	this.test=function(name,fn){
		//页面中result的标签
		result=document.getElementById('result');
		//result为新建的ul标签
		result=assert(true,name).appendChild(document.createElement('ul'));
		//回调函数执行子断言，封装在ul标签里面
		fn();
	}

})();	
test('A test',function(){
	assert(true,'one of A test!');
	assert(false,'one2 of A test!');
	assert(true,'one2 of A test!');
});
test('B test',function(){
	assert(true,'one of A test!');
	assert(false,'one2 of A test!');
	assert(true,'one2 of A test!');
});

</code></pre>
