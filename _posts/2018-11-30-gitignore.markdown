---
layout: post
title: Git 选择性忽略上传
date: 2018-11-30 00:00:00 +0300
description: 解决项目项目中部分分享问题
tags: [Git]
---
## 如何选择性上传GitHub?

在准备分享的这段时间里、出现了很多的问题每次更新了代码 引用了新的组件都会重新添加到node_modules文件夹中，而有一些涉及到公司的私有npm库不方便上传到公共库。如何选择性的上传文件？

我们再上传项目代码时，会发现会有node_modules文件夹 以及 .idea git自动生成的缓存文件夹等如何选择忽视他们呢 我们直接上代码。


目录结构
webpack
- .idea
- build
- config
- node_modules
- pages
- package.json
- .gitignore
- ...



.gitignore 文件
```
/node_modules
/.idea
```

### 注意
如果已经上传后需要 git rm -r node_modules 删除后重新上传 否则.gitignore配置不生效