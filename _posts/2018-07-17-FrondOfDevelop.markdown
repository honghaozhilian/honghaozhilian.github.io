---
layout:     post
title:      "Front Of Develop"
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
1. [Doctype作用](#Doctype作用)
2. [link和@import有什么区别](#link和@import有什么区别)
3. [区分 HTML 和 HTML5](#区分 HTML 和 HTML5)

## Doctype作用
  1. <!DOCTYPE>声明位于HTML文档中的第一行，处于 <html> 标签之前。告知浏览器的解析器用什么文档标准解析这个文档。DOCTYPE不存在或格式不正确会导致文档以兼容模式呈现。
  2. 标准模式与兼容模式：标准模式的排版 和JS运作模式都是以该浏览器支持的最高标准运行。在兼容模式中，页面以宽松的向后兼容的方式显示,模拟老式浏览器的行为以防止站点无法工作。
  3. 在HTML4.01中<!doctype>声明指向一个DTD，由于HTML4.01基于SGML，所以DTD指定了标记规则以保证浏览器正确渲染内容，HTML5不基于SGML，所以不用指定DTD

## link和@import有什么区别
  1. link属于XHTML标签，除了加载CSS外，还能用于定义RSS, 定义rel连接属性等作用；而@import是CSS提供的，只能用于加载CSS;
  2. 页面被加载的时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载;
  3. import是CSS2.1 提出的，只在IE5以上才能被识别，而link是XHTML标签，无兼容问题;

## 区分 HTML 和 HTML5
  1. DOCTYPE声明\新增的结构元素\功能元素
  2. 支持HTML5新标签：IE8/IE7/IE6支持通过document.createElement方法产生的标签,成熟的框架、比如html5shim;
```html
<!--[if lt IE 9]>
  <script> src="http://html5shim.googlecode.com/svn/trunk/html5.js"</script>
<![endif]-->
```
---



