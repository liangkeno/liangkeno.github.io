---
layout: post
category : tools
title: "前端可视化工具"
tags : [livestyle, livereload,browser-sync]
---
{% include JB/setup %}

>- 使用前端可视化工具可实时在浏览器看到调试效果，此次仅作为学习笔记，仅防忘记



**可视化工具 livestyle**

>- livestyle作为chrome浏览器的插件，主要用于调试页面样式，可双向，在浏览器的更改可反应到编辑器上，在编辑器的更改可实时反应到浏览器上，面向小型应用的可视化调试
>- 可在chrome扩展上搜livestyle添加插件；在sublime text 上ctrl+shift+p，install packgeage, 在安装livestyle包；
>- 安装livestyle app
>- 可在项目目录下使用node npm安装http-server包，命令行启http-server启动当前目录的本地服务器
>- 使用sass样式预编译,npm 安装 **npm install sass -g**,使用命令**sass --watch “./ : ./”**监听并编译成css文件
>- 官网：http://livestyle.io/

![](http://i.imgur.com/5aURqHO.jpg)

**可视化工具 livereload**

>- livereload 在保存文件时，可用于自动刷新浏览器中的html与css 效果
>- 需要安装chrome浏览器插件，同时用npm 安装 livereload 包，
>- 启动：在另一个窗口启动http-server,在浏览器工具栏上启动插件，同时在node上使用命令行 livereload ,在当前目录上启动监听
>- 可配合gulp使用，需要安装gulp构建工具及gulp-livereload,编写如下gulpfile.js配置文件，命令行gulp watch启动监听
<pre><code>
var gulp = require('gulp');
var livereload = require('gulp-livereload');
gulp.task('watch', function() {
	livereload.listen();

	gulp.watch('./*.html', function(file) {
		console.log(file);
		gulp.src(file.path).pipe(livereload());
	})
})

</code></pre>

**可视化工具 browser-sync**

>- browser-sync 应用场景比较多，可实时监控html，css等，不需要启动http-server,不需要安装浏览器插件，可与gulp配合使用，
>- 可实现交互同步，文件同步，移动，桌面前端效果同步等
>- 使用命令行启动监听，browser-sync start --server --files "**"，监听某个目录下的所有文件，尽量不要整个静态网站，否则性能不好，可监听于前端某个文件夹
>- 配合gulp使用，安装gulp,browser-sync包，配置gulpfile.js文件，使用命令行启动监听：gulp browser-sync
>- 移动端可在浏览器使用本地电脑ip加端口访问 
>- 官网：https://www.browsersync.io/
<pre><code>
var gulp = require('gulp');
var browsersync = require('browser-sync');

gulp.task('browser-sync', function() {
	browsersync.init({
		server: {
			baseDir: "./"
		}
	})
})

</code></pre>