---
layout:     post
title:      "HTML"
subtitle:   "面试题归纳"
date:       2018-07-17
author:     "honghaozhilian"
header-img: "img/post-bg-js-module.jpg"
tags:
    - 前端开发
---



## html

> 主要是html相关的题目

## 目录
1. [doctype作用](#doctype作用)
2. [link和@import有什么区别](#link和@import有什么区别)
3. [区分html和html5](#区分html和html5)
4. [什么是web语义化以及有什么好处](#什么是web语义化以及有什么好处)
5. [iframe有那些缺点](#iframe有那些缺点)
6. [html5的form如何关闭自动完成功能](#html5的form如何关闭自动完成功能)
7. [优雅降级和渐进增强](#优雅降级和渐进增强)
8. [页面可见性可以有哪些用途](#页面可见性可以有哪些用途)

## doctype作用
>  1. <!DOCTYPE>声明位于HTML文档中的第一行，处于 <html> 标签之前。告知浏览器的解析器用什么文档标准解析这个文档。DOCTYPE不存在或格式不正确会导致文档以兼容模式呈现。
>  2. 标准模式与兼容模式：标准模式的排版 和JS运作模式都是以该浏览器支持的最高标准运行。在兼容模式中，页面以宽松的向后兼容的方式显示,模拟老式浏览器的行为以防止站点无法工作。
>  3. 在HTML4.01中<!doctype>声明指向一个DTD，由于HTML4.01基于SGML，所以DTD指定了标记规则以保证浏览器正确渲染内容，HTML5不基于SGML，所以不用指定DTD

## link和@import有什么区别
>  1. link属于XHTML标签，除了加载CSS外，还能用于定义RSS, 定义rel连接属性等作用；而@import是CSS提供的，只能用于加载CSS;
>  2. 页面被加载的时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载;
>  3. import是CSS2.1 提出的，只在IE5以上才能被识别，而link是XHTML标签，无兼容问题;

## 区分html和html5
>  1. DOCTYPE声明\新增的结构元素\功能元素
>  2. 支持HTML5新标签：IE8/IE7/IE6支持通过document.createElement方法产生的标签,成熟的框架、比如html5shim;
```html
  <!--[if lt IE 9]> 
      <script> src="http://html5shim.googlecode.com/svn/trunk/html5.js"</script> 
  <![endif]--> 
```

## 什么是web语义化以及有什么好处
>  web语义化是指通过HTML标记表示页面包含的信息，包含了HTML标签的语义化和css命名的语义化。 HTML标签的语义化是指：通过使用包含语义的标签（如h1-h6）恰当地表示文档结构 css命名的语义化是指：为html标签添加有意义的class，id补充未表达的语义
>  1. 去掉样式后页面呈现清晰的结构
>  2. 更容易被无障碍设备识别
>  3. 搜索引擎更好地理解页面，有利于收录
>  4. 方便团队项目的可持续运作及维护

## iframe有那些缺点
>  iframe会阻塞主页面的Onload事件；
>  搜索引擎的检索程序无法解读这种页面，不利于SEO;
>  iframe和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载。
>  使用iframe之前需要考虑这两个缺点。如果需要使用iframe，最好是通过javascript
>  动态给iframe添加src属性值，这样可以绕开以上两个问题。

## html5的form如何关闭自动完成功能
>  给不想要提示的 form 或某个 input 设置为 autocomplete=off

## 优雅降级和渐进增强
> - 优雅降级：Web站点在所有新式浏览器中都能正常工作，如果用户使用的是老式浏览器，则代码会针对旧版本的IE进行降级处理了,使之在旧式浏览器上以某种形式降级体验却不至于完全不能用。
> - 渐进增强：从被所有浏览器支持的基本功能开始，逐步地添加那些只有新版本浏览器才支持的功能,向页面增加不影响基础浏览器的额外样式和功能的。当浏览器支持时，它们会自动地呈现出来并发挥作用。
  如：默认使用flash上传，但如果浏览器支持 HTML5 的文件上传功能，则使用HTML5实现更好的体验；

## 页面可见性可以有哪些用途
> 通过 visibilityState 的值检测页面当前是否可见，以及打开网页的时间等;在页面被切换到其他后台进程的时候，自动暂停音乐或视频的播放；

---



