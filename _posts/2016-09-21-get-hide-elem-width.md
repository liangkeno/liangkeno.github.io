---
layout: post
category : javascript
title: "获取一个display为none的元素宽高"
tags : [offset, js技巧]
---
{% include JB/setup %}



>- 在页面上如果把一个元素设置为display:none,以后在元素会被隐藏同时在页面上不占据空间，
>同时使用js得不到其宽高；设置为visibility:hide时，元素会被隐藏，但占据空间；如果元素在hide的情况下使其浮动，
>可以达到即可隐藏元素，同时可以获取其宽高
>- **实现过程**可以先把元素的出初始样式：display,visibility,position保存起来，同时给这三个样式属性设置新的属性：block,hide,absolute;
>接着使用offsetWidth,offsetHeight,获取宽高，得到结果以后，把初始样式恢复。

**定义一个div为none的隐藏元素**
<pre><code>.div{
	width: 50px;
	height: 50px;
	padding: 10px;
	border:10px solid #ccc;
	box-sizing: border-box;
	display: none;
}</code>
</pre>

**使用javascript获取其宽高**

<pre>
window.onload = function() {
	var Container = function(elem) {
		var properties, previous, result;
		//elem元素可获取尺寸样式的属性
		properties = {
			position: 'absolute',
			visibility: 'hidden',
			display: 'block'
		}
		previous = {};
		//对elme设置可获取宽高的样式
		for (var key in properties) {
			previous[key] = elem.style[key];
			elem.style[key] = properties[key];
		}
		//获取宽高
		result = {
				width: elem.offsetWidth,
				height: elem.offsetHeight
			}
			//恢复原来的样式
		for (var key in properties) {
			elem.style[key] = previous[key];
		}
		return result;
	}
	var div = document.getElementsByTagName('div')[0];
	var offset = Container(div);
	console.log(offset);
}

</pre>
**执行结果：Object {width: 50, height: 50}**
