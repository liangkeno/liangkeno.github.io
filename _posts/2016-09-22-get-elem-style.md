---
layout: post
category : javascript
title: "一个兼容老版浏览器获取Dom对象样式的小函数"
tags : [getComputedStyle, currentStyle]
---
{% include JB/setup %}



>- **计算样式**是应用在元素的上的所有样式组合，包括内联，样式表，js设置的样式
>
>- 在兼容性方面，现代浏览器可使用getComputedStyle方式获取（包括Ie9）,对于Ie9之前的版本可使用dom的专有属性currentStyle获取
>
>- 通过getComputedStyle方式返回一个可以进行属性查询的接口getPropertyValue(),接受正常的样式属性名作为参数，调用之前可过滤驼峰命名为正常样式名
>- 通过currentStyle[attr]方式，attr为驼峰名的样式名，调用之前可转换正常样式名为驼峰命名
>
**实现代码如下**
<pre>
//css
<code>div{border:2px solid #ccc;font-size: 20px;}</code>
//div
&lt;div style=&quot;font-size:30px;&quot;&gt;获取元素样式测试&lt;/div&gt;
</pre>

**使用javascript获取其宽高**

<pre>
(function(){
	window.computedStyle=function(elem,property){
		//现代浏览器获取方式(包括Ie9)
		var styles=window.getComputedStyle(elem);
		if(styles){
			//过滤驼峰命名的样式属性名为正常属性名
			property=property.replace(/([A-Z])/g,'-$1').toLowerCase();
			return styles.getPropertyValue(property);

		}else if(elem.currentStyle){
			//过滤正常的样式属性名为驼峰命名
			property=property.replace(/-([a-z])/ig,function(all,letter){
				return letter.toUpperCase();
			});
			return elem.currentStyle[property]
		}
	}
})();
var div=document.getElementsByTagName('div')[0];
//通过正常样式名获取计算样式
var style1=computedStyle(div,'border-top');
//通过驼峰样式名获取计算样式
var style2=computedStyle(div,'fontSize');
console.log(style1);//2px solid rgb(204, 204, 204)
console.log(style2);//30px

</pre>

