---
layout: post
title: npm install 指令的几种使用区别
date: 2018-12-19 00:00:00 +0300
description: npm install ? npm install -g ? npm install --save ? npm install --save-dev
tags: [npm]
---

1.npm install本地安装
（1）将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录。
（2）可以通过 require() 来引入本地安装的包。
2.npm install -g全局安装
(1) 将安装包放在 /usr/local 下或者你 node 的安装目录。
(2)可以直接在命令行里使用。
3.npm install --save
(1)会把msbuild包安装到node_modules目录中
(2)会在package.json的dependencies属性下添加msbuild
(3)之后运行npm install命令时，会自动安装msbuild到node_modules目录中
(4)之后运行npm install --production或者注明NODE_ENV变量值为production时，会自动安装msbuild到node_modules目录中
4.npm install --save-dev
(1)会把msbuild包安装到node_modules目录中
(2)会在package.json的devDependencies属性下添加msbuild
(3)之后运行npm install命令时，会自动安装msbuild到node_modules目录中
(4)之后运行npm install --production或者注明NODE_ENV变量值为production时，不会自动安装msbuild到node_modules目录中