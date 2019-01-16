---
layout: post
title: 内存泄露
date: 2019-01-16 00:00:00 +0300
description: 前端常见的内存泄露的几种原因
img: leak.jpg
tags: [WEB]
---

## 什么是内存泄露？
> 程序的运行需要内存，而只要程序提出要求，操作系统或者运行时(runtime)就必须提供内存。而不再用到的内存，并且没有及时得到释放 称为内存泄露

## 什么是内存溢出
> 程序要求的内存超出了系统所分配的范围  例如用一个int型 4字节的数据来盛放float型8字节的数据就会内存溢出

## 常见的内存泄漏原因

1. 全局变量引起的内存泄漏

```
function leaks(){  
    leak = 'xxxxxx';    //leak 成为一个全局变量，不会被回收
}
```
> 我们为了确保避免内存泄漏在使用完全局变量后 应将其设置为 null

2. 闭包引起的内存泄漏

```
var leaks = (function(){  
    var leak = 'xxxxxx';// 被闭包所引用，不会被回收
    return function(){
        console.log(leak);
    }
})()

```

3. dom清空或者删除时，事件未清除

```
<div id="container"></div>
 
$('#container').bind('click', function(){
    console.log('click');
}).remove();

//导致内存泄漏
```

```
<div id="container"></div>
 
$('#container').bind('click', function(){
    console.log('click');
}).off('click').remove();
 
//把事件清除了，即可从内存中移除

```

> 子元素存在引用引起的内存泄漏: 假如你js代码中保存了td标签 删除整个表格的时候 直觉上认为会删除除td以外的全部结构 其实不然 td和父元素是引用关系由于保留了td 所以整个表格都将保存到内存中

4. 计时器或回调函数

```
var someResource = getData(); 
setInterval(function() { 
    var node = document.getElementById('Node'); 
    if(node) { 
        // 处理 node 和 someResource 
        node.innerHTML = JSON.stringify(someResource)); 
    } 
}, 1000); 
```


## 如何避免内存泄露
* 减少不必要的全局变量，使用完置为 null
* 开发过程中注意逻辑避免出现 ’死循环‘
* 元素绑定时间&定时器要及时clear