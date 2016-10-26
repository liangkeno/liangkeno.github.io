---
layout: post
category : react
title: "使用es6的class重构react组件"
tags : [react, 总结,es6 class]
---
{% include JB/setup %}

### 安装yeoman中的react-webpack脚手架

<pre><code>
npm install -g yo
npm install -g generator-react-webpack

</code></pre>

### 初始化脚手架

<pre><code>
//使用淘宝镜像
cnpm install

</code></pre>


### 创建项目

<pre><code>
mkdir es6-component && cd es6-component
yo react-webpack

</code></pre>

### 创建组件
>此时会在components 创建NewComponent.js;创建styles/New.css;创建测试文件test/component/NewComponentTest.js;三个文件
<pre><code>
yo react-webpack:component new

</code></pre>


### 打开components/NewComponent.js编写组件

<pre><code>
'use strict';

import React from 'react';
require('styles//New.css');

class NewComponent extends React.Component {
	constructor(props) {
			//初始化时，传递props给React.Component
			super(props);
			//初始化状态，也在此定义
			this.state = {
				assert: 'yes'
			};
		}
		//处理单击的事件，绑定dom对象在render标签中使用bind注入
	handleClick() {
			alert('you click me');
		}
		//组件自身的render
	render() {
		return (
		&lt;div className=&quot;new-component&quot;&gt;
		    {this.props.name}
		    &lt;button onClick={this.handleClick.bind(this)} &gt;click me&lt;/button&gt;
		&lt;/div&gt;
		);
	}
}
//定义属性在class外部定义
NewComponent.propTypes = {
	name: React.PropTypes.string
};
//默认属性方法也以class属性方式表示
NewComponent.defaultProps = {
	name: 'hello world!'
};
NewComponent.displayName = 'NewComponent';

// Uncomment properties you need
// NewComponent.propTypes = {};
// NewComponent.defaultProps = {};

//在此导出newComponent组件，在某地方输出组件时使用
//import New from './components/NewComponent'; 引入
export default NewComponent;

</code></pre>

###运行脚手架
<pre><code>
npm start

</code></pre>


[yeoman react-webpack 文档说明](https://github.com/react-webpack-generators/generator-react-webpack#readme )