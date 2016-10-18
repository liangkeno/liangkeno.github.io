---
layout: post
category : tools
title: "学习react的总结笔记"
tags : [react, 总结]
---
{% include JB/setup %}

### demo引入的库

<pre><code>
&lt;script type=&quot;text/javascript&quot; src=&#39;../build/react.js&#39;&gt;&lt;/script&gt;
&lt;script type=&quot;text/javascript&quot; src=&#39;../build/react-dom.js&#39;&gt;&lt;/script&gt;
&lt;script type=&quot;text/javascript&quot; src=&#39;../build/browser.min.js&#39;&gt;&lt;/script&gt;

</code></pre>

>包括三个库，react.js核心库，react-dom.js提供与相关的功能,browser.js将jsx语法转javascript

### 初识

>调用ReactDOM.render()方法，往里传入标签和要插入页面dom的两个参数
<pre><code>
ReactDOM.render(
  &lt;h1&gt;Hello, world!&lt;/h1&gt;,
  document.getElementById('example')
);

</code></pre>

### jsx语法

>- 对jsx的解析，遇到html标签就使用html规则进行解析，遇到代码块 { 就使用javascript进行解析；
>- 新建一个数组，在render函数第一个参数中传入以div包裹的代码块，代码块使用map遍历数组，对每个元素执行回调，返回封装的标签
<pre><code>
var names = ['Alice', 'Emily', 'Kate'];
ReactDOM.render(
&lt;div&gt; {
    names.map(function (name) {
      return &lt;div&gt;Hello, {name}!&lt;/div&gt;
    })
  }&lt;/div&gt;,
  document.getElementById('example')
);

</code></pre>

### 组件 

>- 使用React.createClass()生成组件，传入一个对象参数，定义一个render方法，每个组件都又自己的一个render方法，返回并赋值给HelloMessage变量
>- 变量HelloMessage，首字母必须为大写否则出错，相当于类的命名，字母首个大写；
>- 传入React.DOM.render的参数一是以变量HelloMessage转为html格式传入，里面可定义html属性，在组件的render方法内可以使用this.props对象访问，render函数返回的标签只能包含一个顶层标签；
>- 在添加组件属性时，需注意class属性写成className,for属性写成htmlFor,因为class,for都是javascript的保留字；
<pre><code>
var HelloMessage = React.createClass({
  render: function() {
    return &lt;h1&gt;Hello {this.props.name}&lt;/h1&gt;;
  }
});

ReactDOM.render(
  &lt;HelloMessage name=&quot;John&quot; /&gt;,
  document.getElementById('example')
);

</code></pre>

### 特例的属性this.props.children

>- React.Children为顶层Api之一，为处理this.props.children数据结构的工具，包括5个方法：map,count,forEach,toArray,only
>- this.props的属性与组件的属性一一对应，this.props.children属性例外，用于表示组件的所有子节点;
>- this.props.children的值有三种可能，子节点为0个，值为undefined；子节点为一个，值为object;子节点为多个，值为array;
<pre><code>
//创建NodeList组件
var NodeList=React.createClass({
	//定义组件的render方法
	render:function(){
		//存储NodeList对象
		var self=this;
		console.log(React.Children);
		return (
			//顶层标签
			&lt;ol&gt;
				//代码块：使用React.Children.map遍历组件的所有子节点
				{
				React.Children.map(this.props.children,function(child){
					return &lt;li&gt;{self.props.greetting+&quot; &quot;}{child}&lt;/li&gt;
				})}
			&lt;/ol&gt;
		)
	}
});
ReactDOM.render(
	&lt;NodeList greetting=&quot;hello&quot;&gt;
		&lt;span&gt;item1&lt;/span&gt;
		&lt;span&gt;item2&lt;/span&gt;
		&lt;span&gt;item2&lt;/span&gt;
	&lt;/NodeList&gt;,
	document.getElementById(&#39;demo&#39;)
);

</code></pre>

### 设置组件属性的类型PropTypes

>组件的属性可以接受字符串，数值，对象，函数，使用PropTypes可以验证组件的属性的类型，当验证类型不通过时，会在控制台输出错误；
<pre><code>
var Mytitle=React.createClass({
	propTypes:{
		title:React.PropTypes.string.isRequired,
	},
	render:function(){
		return &lt;h1&gt;{this.props.title}&lt;/h1&gt;
	}
});
var data=123;
ReactDOM.render(
	&lt;Mytitle title={data} /&gt;,
	document.getElementById(&#39;demo&#39;)
);

</code></pre>

### 获取真实的DOM

>- 组件并不是真实的dom节点，只是存在内存中的一种虚拟的数据结构，叫做虚拟的DOM；
>- 在组件标识ref属性，在事件回调函数中通过this.refs.[refName]获取真实dom节点；
>- 由于this.refs.[refName]获取的时真实的dom,所以必须等到虚拟DOM插入文档后才能使用这个属性，否则报错；

<pre><code>
var Mycomponet=React.createClass({
	handleClick:function(){
		//通过refs对象获取在组件标识的真实dom对象
		this.refs.realDom.focus();
	},
	render:function(){
		return (
			&lt;div&gt;
				//input中定义一个ref属性，方便回调函数获取真实dom
				&lt;input type=&quot;text&quot; ref=&quot;realDom&quot; /&gt;
				&lt;input type=&quot;button&quot; value=&quot;按钮&quot; onClick={this.handleClick} /&gt;			
			&lt;/div&gt;
		);
	}
});
ReactDOM.render(
	&lt;Mycomponet /&gt;,
	document.getElementById(&#39;demo&#39;)
);

</code></pre>

### this.state

>- 一个组件可相当一个状态机，有初始状态，当与用户交互时，导致状态发生改变，从而触发render，重新渲染；
>- this.getInitialState设置初始状态；this.state获取状态；this.setState修改状态，从触发render；
>- this.props与this.state都是描述组件的特性，区别是this.props描述的是定义就不改变的特性，this.state描述的是随用户交互而改变的特性
<pre><code>
var LikeBtn=React.createClass({
	//设置初始的state,可通过this.state.[attr]获取状态
	getInitialState:function(){
		return {liked:false}
	},
	//单击事件，通过this.setState修改状态，状态改变自动调用render渲染组件
	handleClick:function(event){
		this.setState({liked:!this.state.liked});
	},
	render:function(){
	var text = this.state.liked ? &#39;like&#39; : &#39;haven\&#39;t liked&#39;;
	    return (
	      &lt;p onClick={this.handleClick}&gt;
	        You {text} this. Click to toggle.
	      &lt;/p&gt;
	    );
	}
});
ReactDOM.render(
	&lt;LikeBtn /&gt;,
	document.getElementById(&#39;demo&#39;)
);

</code></pre>

### 表单中的特性

>- 通过表单与用户交互，在交互的过程中触发事件，改变组件状态，实时渲染组件UI
>- 定义一个输入文本框，监听change事件，当值改变时，通过事件改变组件状态，重新渲染组件
<pre><code>
var Input=React.createClass({
	getInitialState:function(){
		return {value:&#39;hello!&#39;};
	},
	handleChange:function(event){
		//通过用户输入的值改变状态，进行渲染
		this.setState({value:event.target.value});
	},
	render:function(){
		var value=this.state.value;
		return(
			&lt;div&gt;
				//数据块两边不能加引号，自个调试容易犯的错 value=&quot;{value}&quot;
				&lt;input type=&quot;text&quot; name=&quot;input&quot; value={value} onChange={this.handleChange} /&gt;
				&lt;p&gt;{value}&lt;/p&gt;
			&lt;/div&gt;
		);
	}
});
ReactDOM.render(&lt;Input /&gt;,document.getElementById(&#39;demo&#39;));

</code></pre>

### 组件的声明周期

>- 组件生命周期的状态包括：Mounting已经插入真实的dom；Updating正在被重新渲染；Unmounting已经移除真实Dom;
>- 每种状态提供两种处理函数，will进入状态前调用，did进入状态后调用；
>- 三种状态定义五个函数，componentWillMount,componentDidMount,
>componentWillUpdate,componentDidUpdate,componentWillUnmount
>- 两个特殊方法：componentWillReceiveProps（收到新参数调用），shouldComponentUpdate
<pre><code>
var Input=React.createClass({
	getInitialState:function(){
		return {opacity:1.0};
	},
	//插入真实dom后执行
	componentDidMount:function(){
		var timer=setInterval(function(){
			var opacity=this.state.opacity;
			opacity=opacity-0.05;
			if(opacity&lt;0.1){
				opacity=1.0;
			}
			//改变了state,重新渲染组件，接着往复执行componentDidMount
			this.setState({opacity:opacity});
		}.bind(this),100);
	},
	render:function(){
		var opacity=this.state.opacity;
		return(
			&lt;div&gt;
		//css样式为对象，第一个花括号为了jsx解析，第二个说明这事css对象
			&lt;p style={{opacity}}&gt;hello world!&lt;/p&gt;
			&lt;/div&gt;
		);
	}
});
ReactDOM.render(&lt;Input /&gt;,document.getElementById(&#39;demo&#39;));

</code></pre>

*参考资料*

[阮一峰 React 入门实例教程](http://www.ruanyifeng.com/blog/2015/03/react.html )