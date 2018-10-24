---
layout: post
title: 滚动条样式修改方式
date: 2018-10-24
description: 记录一下开发过程中 需要修改滚动条、样式的代码方法
tags: [CSS]
---

## 隐藏滚动条 

```
::-webkit-scrollbar {
  display : none ;
}
```

## 更改滚动条样式

```
::-webkit-scrollbar {   //更改样式必须要这一条！
  width: 10px;
}
::-webkit-scrollbar-thumb { //滑动块样式

}
::-webkit-scrollbar-track { //轨道样式

}
```

## 详细属性
::-webkit-scrollbar 滚动条整体部分
::-webkit-scrollbar-thumb  滚动条里面的小方块，能向上向下移动（或往左往右移动，取决于是垂直滚动条还是水平滚动条）
::-webkit-scrollbar-track  滚动条的轨道（里面装有Thumb）
::-webkit-scrollbar-button 滚动条的轨道的两端按钮，允许通过点击微调小方块的位置。
::-webkit-scrollbar-track-piece 内层轨道，滚动条中间部分（除去）
::-webkit-scrollbar-corner 边角，即两个滚动条的交汇处
::-webkit-resizer 两个滚动条的交汇处上用于通过拖动调整元素大小的小控件

## demo 

```
::-webkit-scrollbar {
    width: 10px;   
    height: 1px;
}
::-webkit-scrollbar-thumb {
    border-radius: 10px;
    background-color: #F90;
    background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, .2) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, .2) 50%, rgba(255, 255, 255, .2) 75%, transparent 75%, transparent);
}
::-webkit-scrollbar-track {
    -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
    background: #EDEDED;
}
```