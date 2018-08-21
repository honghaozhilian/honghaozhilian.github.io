---
layout:     post
title:      "前端框架"
subtitle:   "面试题归纳"
date:       2018-07-30
author:     "honghaozhilian"
header-img: "img/post-bg-js-module.jpg"
tags:
    - 前端开发
---

## 前端框架

> 主要是前端的框架的相关问题

## 目录
- [前端路由的实现原理](#前端路由的实现原理)
- [react和angular以及vue的设计思想](#react和angular以及vue的设计思想)
- [react的单向数据原理与vue的双向数据绑定原理](#react的单向数据原理与vue的双向数据绑定原理)
- [react与vue各自的组件生命周期](#react与vue的组件生命周期)
- [react与vue各自的组件通信](#react与vue各自的组件通信)
- [react的事件绑定this的解决方案](#react的事件绑定this的解决方案)
- [react的批量更新机制](#react的批量更新机制)
- [react的优化原理](#react的优化原理)
- [react_router3和react_router4的区别](#react_router3和react_router4的区别)
- [redux的优缺点](#redux的优缺点)
- [react_redux与context](#react_redux与context)
- [redux是如何做到可预测呢](#redux是如何做到可预测呢)
- [redux将react组件划分为哪两种](#redux将react组件划分为哪两种)
- [redux是如何将state注入到react组件上的](#redux是如何将state注入到react组件上的)
- [什么是高阶组件及应用场景](#什么是高阶组件及应用场景)
- [受控组件与非受控组件](#受控组件与非受控组件)
- [vue的computed的依赖缓存](#vue的computed的依赖缓存)
- [angular的脏值检测原理](#angular的脏值检测原理)
- [backbone的mvc实现方式](#backbone的mvc实现方式)
- [webpack热更新实现原理](#webpack热更新实现原理)
- [webpack的loader的原理](#webpack的loader的原理)

## 前端路由的实现原理

## react和angular以及vue的设计思想

## react的单向数据原理与vue的双向数据绑定原理

## react与vue各自的组件生命周期

## react与vue各自的组件通信

## react的事件绑定this的解决方案

## react的批量更新机制

## react的优化原理

## react_router3和react_router4的区别

## redux的优缺点

## react_redux与context

## redux是如何做到可预测呢

## redux将react组件划分为哪两种

## redux是如何将state注入到react组件上的

## 什么是高阶组件及应用场景

## 受控组件与非受控组件

## vue的computed的依赖缓存


## angular的脏值检测原理

## webpack的loader的原理

## backbone的mvc实现方式

## webpack热更新实现原理
1. Webpack编译期，为需要热更新的 entry 注入热更新代码(EventSource通信)
2. 页面首次打开后，服务端与客户端通过 EventSource 建立通信渠道，把下一次的 hash 返回前端
3. 客户端获取到hash，这个hash将作为下一次请求服务端 hot-update.js 和 hot-update.json的hash
4. 修改页面代码后，Webpack 监听到文件修改后，开始编译，编译完成后，发送 build 消息给客户端
5. 客户端获取到hash，成功后客户端构造hot-update.js script链接，然后插入主文档
6. hot-update.js 插入成功后，执行hotAPI 的 createRecord 和 reload方法，获取到 Vue 组件的 render方法，重新 render 组件， 继而实现 UI 无刷新更新。

## webpack的loader的原理



---
