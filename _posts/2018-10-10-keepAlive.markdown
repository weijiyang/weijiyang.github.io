---
layout: post
title: keepAlive的学习理解
date: 2018-10-10 13:32:20 +0300
description: 介绍keepAlive组件使用的场景及简单案例
tags: [Vue]
---
使用keep-alive组件 进入页面经历的过程是 created -》mounted -》activated 当离开时会触发deactivated 再次进入时只会触发activated函数

所以如果每次进入需要更新对应代码的话 需要将函数接口移至activated内部
如果是通过$route跳转的话 ，需要在activated函数中重新对其参数重新复制data

属性 prop
include ： 字符串或者正则表达式 只有匹配的组件才会被缓存
exclude ： 字符换或者正则表达式 匹配到的组件都不会被缓存

keep-alive的几种使用形式：
1. 父组件中keep-alive匹配组件
![web server show html]({{site.baseurl}}/assets/img/2018.10.10/2018.10.10_keepAlive1.png)
2. 结合$route缓存部分页面
使用$route.meta 中的keepAlive属性
![web server show html]({{site.baseurl}}/assets/img/2018.10.10/2018.10.10_keepAlive2.png)
也有可能会出现我们路由间相互跳转时分不同情况来决定是否缓存页面 这个时候我们就可以使用第二种与$route结合的方式  在beforeRouteLeave钩子函数中设置to.meta.keepAlive的值  
