---
layout: post
title: requestAnimationFrame
date: 2018-10-10 13:32:20 +0300
description: requestAnimationFrame、setTimeout、setInterval区别
tags: [requestAnimationFrame]
---

* 商城中使用动画用的是requestAnimationFrame API 
我们一般使用的是setTimeout 和setInterval这两个方法，但是这两个方法存在一定的问题：
1. setTimeout、setInterval不够精准他们内在的运行机制传递的时间间隔参数只能决定在浏览器UI线程队列中的等待执行时间如果队列前面已经加入了其他任务 那么他们的执行会放到其他任务执行之后
2. requestAnimationFrame采用系统的时间间隔时间，保证最佳绘画效率节省资源的同时能够优化动画视觉效果 （1.对于隐藏或不可见的元素，requestAnimationFrame不会重绘2.页面如果不是激活状态 动画会自动暂停这些都是setTimeout setInterval无法办到的）
3. 但是requestAnimationFrame只支持ie9以上所以兼容性处理
```
if (!window.requestAnimationFrame) {
    requestAnimationFrame = function(fn) {
        setTimeout(fn, 17);
    };    
}
```
