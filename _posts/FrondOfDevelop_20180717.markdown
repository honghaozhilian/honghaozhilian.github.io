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



## 综合题

> 主要是一些计算机基础题，http,面向对象等的题目




---

```js
// js 代码

var a = require('./a')  // 加载模块（同步加载）
a.doSomething()         // 等上一句执行完才会执行

exports.b = function(){ // 暴露 b 函数接口
  // do something
}
```

