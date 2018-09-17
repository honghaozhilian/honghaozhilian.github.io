---
layout:     post
title:      "移动端知识"
subtitle:   "面试题归纳"
date:       2018-07-20
author:     "honghaozhilian"
header-img: "img/post-bg-js-module.jpg"
tags:
    - 前端开发
---

## 移动端知识

> 主要是移动端的知识

## 目录

- [从打开app到刷新出内容整个过程中都发生了什么](#从打开app到刷新出内容整个过程中都发生了什么)
- [zepto的点透问题如何解决](#zepto的点透问题如何解决)
- [移动端300ms延迟的原因及解决方案](#移动端300ms延迟的原因及解决方案)
- [移动端最小触控区域是多大](#移动端最小触控区域是多大)
- [针对手机高清屏做不同图片处理](#针对手机高清屏做不同图片处理)
- [高清屏的border为1px会显示为2px](#高清屏的border为1px会显示为2px)
- [元素设置的position和float后的display的变化](#)

## 从打开app到刷新出内容整个过程中都发生了什么

## zepto的点透问题如何解决
- 当A/B两个层上下z轴重叠，上层的A点击后消失或移开（这一点很重要），并且B元素本身有默认click事件（如a标签）或绑定了click事件。在这种情况下，点击A/B重叠的部分，就会出现点透的现象。
- 原因：zepto的tap事件是通过兼听绑定在document上的touch事件来完成tap事件的模拟的，并且tap事件是冒泡到document上触发的
- 解决方案：
    1. 引入FastClick库
    2. 对于B元素本身存在默认click事件的情况，应及用touchend代替tap事件并阻止掉时A元素touchend的默认行为preventDefault()，从而阻止click事件的产生。
    3. 延迟一定的时间(300ms+)来处理事件

## 移动端300ms延迟的原因及解决方案
- 由于触发点击程序无法判断用户点击后下一步是什么操作，所以有了300ms的延迟
- 解决方案：
   1. FastClick库：检测click事件，然后通过触发自定义事件去代替，并把浏览器在300ms后的触发阻止掉
   2. 用移动端类库的如zeptojs等都支持tap事件来解决
   3. 绑定ontouchstart事件代替
   4. 设置meta的user-scalable,禁止缩放

## 移动端最小触控区域是多大
- 分辨率：手机屏幕的实际像素尺寸(与物理尺寸不同)
- 像素密度(PPI)：每英寸的长度上排列的像素点数量
- 苹果以普通屏为基准，给Retina屏定义了一个2倍的倍率（iPhone 6plus除外，它达到了3倍）。
- Android将像素密度在120左右的屏幕归为ldpi，160左右的归为mdpi,相关倍率如下：
    - ldpi [0.75倍]（已绝迹）
    - mdpi [1倍]（基本不会出现）
    - hdpi [1.5倍]
    - xhdpi [2倍]
    - xxhdpi [3倍]
    - xxxhdpi [4倍]
    <img src="{{ site.baseurl }}/img/dpi.png" alt="dpi"/>
- 真正决定显示效果的，是逻辑像素尺寸。实际像素除以倍率，就得到逻辑像素尺寸。为此，iOS和Android平台都定义了各自的逻辑像素单位。iOS的尺寸单位为pt，Android的尺寸单位为dp。
- 单位之间的换算关系随倍率变化：
    - 1倍：1pt=1dp=1px（mdpi、iPhone 3gs）
    - 1.5倍：1pt=1dp=1.5px（hdpi）
    - 2倍：1pt=1dp=2px（xhdpi、iPhone 4s/5/6）
    - 3倍：1pt=1dp=3px（xxhdpi、iPhone 6 plus）
    - 4倍：1pt=1dp=4px（xxxhdpi）
- Android的最小点击区域尺寸是48x48dp，这就意味着在xhdpi(2倍)的设备上，按钮尺寸至少是96x96px。而在xxhdpi（3倍）设备上，则是144x144px。
- ios为44*44pt，retina屏为88px*88px

## 针对手机高清屏做不同图片处理
- 通过媒体查询的设备像素比判断,加载不同2倍设计图

        @media only screen and (-webkit-min-device-pixel-ratio:1.5){}

- js的window.devicePixelRatio可以判断

## 高清屏的border为1px会显示为2px
通过判断设备像素比:
1. 设置viewport的缩放为0.5
2. 用:after和：before，设置高度1px,transform:scaleY(0.5)缩小一半
3. 用border-image代替

        border-width: 1px;
        border-image: url(border.gif) 2 repeat;
4. 用border-shadow实现：

        border: none;
        box-shadow: 0 1px 1px -1px rgba(0, 0, 0, 0.5);

## 元素设置的position和float后的display的变化
根据[规范](https://www.w3.org/TR/2011/REC-CSS2-20110607/visuren.html#dis-pos-flo)，当设置position为aosolute或fixed,或者float不为none,display根据下表:
    <img src="{{ site.baseurl }}/img/display.png" alt="setState"/>