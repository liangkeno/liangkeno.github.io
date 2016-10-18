---
layout: post
category : tools
title: "记一个简单的webpack.config.js文件看安装的依赖包"
tags : [webpack, demo]
---
{% include JB/setup %}



<pre><code>
module.exports = {
	entry: './basic/app.js',
	output: {
		path: './assets/',
		filename: '[name].bundle.js'
	},
	module: {
		loaders: [{
			//babel-loader加载器，将es6转es5
			test: /\.js$/,
			loader: 'babel'
		}, {
			//style-loader,css-loader加载器，当调用bundle.js
			//时动态生成sytle标签，插入html中的head
			test: /\.css$/,
			loader: 'style!css'
		}]
	}
};
</code></pre>

### package.json 中的安装依赖 


<pre><code>
  "devDependencies": {
    "babel-loader": "^6.2.5",
    "babel-preset-es2015": "^6.16.0",
    "css-loader": "^0.25.0",
    "style-loader": "^0.13.1",
    "webpack": "^1.13.2"
  }

</code></pre>

### 安装过程的坑

<pre><code>
cnpm install webpack --save-dev
cnpm install babel-loader --save-dev
cnpm install css-loader --save-dev
cnpm install style-loader --save-dev

</code></pre>

运行webpack  出现错误，后来查了babel-loader需要安装babel-preset-es2015包依赖

<pre><code>
cnpm install babel-preset-es2015 --save-dev

</code></pre>

### 安装html-webpack-plugin 插件

<pre><code>
cnpm install html-webpack-plugin --save-dev

</code></pre>

> 此插件可以自动生成html文件，可设置模板，生成html文件名，等页面信息

**在webpack.config.js增加如下信息**

<pre><code>
var path = require('path');
//文件头部获取插件类名
var HtmlWebpackPlugin = require('html-webpack-plugin');
......
module.exports = {
//添加插件数组
	plugins: [
		new HtmlWebpackPlugin({
			filename: 'index-release.html',
			title: "hello world",
			template: path.resolve('my-index.ejs'),
			inject: 'body'
		})
	]
}
cnpm install html-webpack-plugin --save-dev
</code></pre>
**新建模板文件 my-index.ejs**

<pre><code>
&lt;!DOCTYPE html&gt;
&lt;html&gt;
  &lt;head&gt;
    &lt;meta http-equiv=&quot;Content-type&quot; content=&quot;text/html; charset=utf-8&quot;/&gt;
    &lt;title&gt;&lt;%= htmlWebpackPlugin.options.title %&gt;&lt;/title&gt;
  &lt;/head&gt;
  &lt;body&gt;
  &lt;/body&gt;
&lt;/html&gt;

</code></pre>