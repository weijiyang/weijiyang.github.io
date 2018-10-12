---
layout: post
title: 对Vue生命周期钩子函数的理解
date: 2018-10-10
description: 官方示例图&个人理解&各钩子函数使用场景
tags: [Vue]
---
对Vue流程图的重新理解：
官方周期函数图
![lifecycle]({{site.baseurl}}/assets/img/2018.10.10/2018.10.10_lifecycle.png)
* beforeCreate 创建前  data & $el均没有初始化 都为undefined
* created 创建后 data初始化 但是$el没有初始化
* beforeMount 载入前 data & $el都初始化 但是dom元素为虚拟dom未装载变量
* mounted 载入后 全度dom元素装载完毕
* beforeUpdate 更新前 在data变量变化渲染之前触发 是渲染前最后的操作机会
* updated 更新后 检测到data变化渲染之后触发
* beforeDestroy 删除前 实例销毁前触发 实例依然有效
* destroyed 删除后 实例销毁之后调用
* activated keep-alive组件激活时调用
* deactivated keep-alive组件停用时调用
![lifecycle]({{site.baseurl}}/assets/img/2018.10.10/2018.10.10_vueImage1.png)
![lifecycle]({{site.baseurl}}/assets/img/2018.10.10/2018.10.10_vueImage2.png)