---
layout: post
title: webpack chunk相关分享
date: 2019-02-21 12:00:00
description: mac环境变量配置
img: webpack.jpeg
fig-caption: mac环境变量配置
tags: [MAC]
---
## webpack chunk相关分享
> 还在为学不会webpack而发愁么，还在因为概念过多而烦恼嘛 欢迎大家来看本次分享

##### 分享内容简介：

1. 什么是webpack(webpack简介、webpack工作流程)
2. 什么是chunk（chunk相关概念、splitChunks插件的使用）
3. webpack 打包优化方案（常用优化插件&打包优化方案）
4. 相关内容扩展（webpack v3 v4区别、hash、scripts配置、path相关概念等）

### 什么是webpack?
![img1]({{site.baseurl}}/assets/img/2018.02.21/1.png)

前端页面效果越来越酷炫、功能越来越复杂
而前端工程师们为了更方便的开发提高开发效率进行了一系列de探索
模块化思想的提出啊 将复杂的程序分割成更小的文件

这些年优秀的框架层出不穷react vue angular
es6这种在javascript基础上拓展的新的语法规范
less sass css处理器等等等

所有的事物都是具有双面性的  有利有弊
大大提高开发效率 的同时 又为后期维护造成了困扰 因为利用这些工具的文件往往不能直接被浏览器识别 需要手动处理 很影响开发进度 而在这一个环境背景下 webpack产生……

我们来看一下官方介绍图

![img2]({{site.baseurl}}/assets/img/2018.02.21/2.png)

webpack可以看做是模块打包机，而它主要做的事情呢就是解析你的代码结构 同时找到javascript模块以及其他的一些浏览器不能直接识别的拓展语言(scss、es6等) 将其转换并打包为合适的格式供浏览器使用

webpack具体为我们做了哪些事情呢 别着急往下看~

![img3]({{site.baseurl}}/assets/img/2018.02.21/3.png)

可以大致分为以下四类：

1. 公共代码抽离没有用到的代码不进行打包 减少整体代码量
2. babel-loader、sass-loader等来处理浏览器不能直接识别的语言 提高开发效率
3. webpack-server 开发模式优化开发流程 减少工作量
4. 文件的混淆压缩 减少页面渲染时间提高系统安全性

使用前后有什么具体的变化么 我们下面详细的举一些例子场景来看

![img4]({{site.baseurl}}/assets/img/2018.02.21/4.png)

| 使用前 | 使用后 |
| :--------: | :--------:|
| 代码过于冗余 维护难度大 | 以入口文件为基点创建关系图 忽略无用代码|
| 相同文件多次请求 资源浪费严重 页面加载速度慢 用户体验差 | 抽离公共chunk 避免重复引用 |
| 开发更改文件 浏览器却优先读取缓存导致无法生效 | 通过hash命名 避免浏览器缓存影响 |
| 代码注释删除繁琐系统安全性没有保障 | 插件清除注释 加密 压缩 混淆一步到位 |
| 开发模式下需要修改后需要频繁回到浏览器刷新查看效果 开发效率低下 | HMR 热更新动态更新视图 避免繁琐操作 |
| 开发技术工具繁多 对开发人员技术要求高 | 丰富的插件、loader自动编译 降低开发难度 |


听到这里 是不是觉得webpack很神奇 而webpack之所以有这些优点其实全是源于其设计理念 我们看一下它的工作流程就会大概明白了他的实现原理了

![img5]({{site.baseurl}}/assets/img/2018.02.21/5.png)

webpack 从入口文件开始递归的建立一个依赖关系图，涉及到的所有文件内容都将被webpack封装为独立的模块函数，按照我们设置的配置文件 进行模块函数的封装打包成一个个chunk，最终通过script标签引入的形式注入到相应的模板视图文件中也就是我们常见的html文件 通过依赖第一步生成的 **manifest** 文件 管理文件的加载运行

我们可控可配置的一般是第二三步骤 我们详细的看一下

![img6]({{site.baseurl}}/assets/img/2018.02.21/6.png)

看完上面的 webpack 打包流程的介绍，是不是对webpack打包过程有了一些理解呢 而我们本次分享的重点，则是学习对chunk有一定的理解和一些自定义化的构建配置


### 什么是chunk?
 `Chunk` 可以简单理解为模块函数的集合也就是“代码块”，通过使用webpack打包处理可以将代码中的多个入口文件、公共引用文件、异步加载文件等抽离成单独的个体、也就是我们打包后产生的一个个文件，我们可以利用chunk来提高 打包效率节省加载时间 更合理的利用浏览器的缓存等…

> 本次分享代码使用 webpack v4版本

通过四个demo来模拟打包处理的四种情况

* [单入口静态文件](https://github.com/meicai-fe/fe-training/blob/master/webpack-chunk/case/config/webpack.case0.js)
* [单入口公共文件](https://github.com/meicai-fe/fe-training/blob/master/webpack-chunk/case/config/webpack.case1.js)
* [单入口异步加载文件](https://github.com/meicai-fe/fe-training/blob/master/webpack-chunk/case/config/webpack.case2.js)
* [多入口文件](https://github.com/meicai-fe/fe-training/blob/master/webpack-chunk/case/config/webpack.case3.js)

文件引用关系图：

![img7]({{site.baseurl}}/assets/img/2018.02.21/7.png)
> 提示：为了方便大家理解，代码使用的是最基本的代码结构 大家可以通过demo了解一下 webpack打包常用插件 clean-webpack-plugin、html-webpack-plugin、webpack-merge、node环境全局变量process.env使用等

我们会发现webpack v4版本默认已经将我们的代码进行一定程度的拆分打包 具体的打包规则如下：

1. 重复引用的chunk和包含来自node_modules文件夹下的模块
2. 在压缩前不小于30kb
3. 按需加载并发请求量不大于5
4. 初始化加载并发请求量不大于3

而这也就涉及到webpack v3 v4版本最大的变化 就是splitChunksPlugin替换commonChunksPlugin
```
module.exports = { //...
   optimization: {
     splitChunks: { 
        chunks: ‘async’ ,                          //可以选择对哪些chunk进行优化（all|async|initial） 
        minSize: 30000, // 要生成的chunk的最小大小(b) 
        minChunks: 1,   //分割前必须共享模块的最小引用次数 
        maxAsyncRequests: 5,    //按需加载 最大并行请求量 
        maxInitialRequests: 3,  //初始化页面 最大并行请求量 
        automaticNameDelimiter: ‘~’,            //chunk一般名称格式为（例如vendors~main.js）该项指分隔符 
        name: true,                                 //chunk的名称 (true时将自动生成名称) 如果名称和入口点名称不相符则删除入口点 
        cacheGroups: {  //缓存组可以继承和覆盖splitChunks.*全部属性 
        vendors: {
            test: /[\\/]node_modules[\\/]/, //test属性只有在cache group中操作 正则匹配chunk放到该缓存组中 
            priority: -10       //priority属性只有在cache group中操作 优先级(默认组优先级为负) }, 
            default: {
                minChunks: 2,
                priority: -20, 
                reuseExistingChunk: true,          //reuseExistingChunk属性只有在cache group操作 是否可以使用已存在的chunk 如果满足条件的chunk已存在就不会再创建新的chunk 
               filename: '[name].bundle.js'     //打包生成文件名称
            }
        }
     } 
   }
};

```

测试上面的配置可以使用代码案例哦~  [这里](https://github.com/meicai-fe/fe-training/blob/master/webpack-chunk/development%26production/webpack.dev.js)

> 这里大家可能和我一样对默认规则的3、4条理解不是很清楚 上面所说的按需并行请求量和初始化并发请求量对应的分别是 maxAsyncRequest 和 maxInitialREquests这两个属性 而这两个属性其实是为了解决打包过于细化 请求量过多的问题 我们如果使用cacheGroups 进行划分有可能会出现很多chunk 而这种情况不是我们所期望的 如果最终chunk数量超过了我们设置的最大值 webpack则会按照一定规则将chunk合并至最大值 如果还不是很清楚的话 就边看页面scripts请求数量 边调最大值看看就懂了 = =

### webpack打包优化方案
1. 减少打包时间（抽离公共库文件） ： `DllPlugin&DllReferencePlugin` [案例](https://github.com/meicai-fe/fe-training/blob/master/webpack-chunk/development%26production/webpack.dll.js)
2. 可视化工具：`webpack-bundle-analyzer` [案例](https://github.com/meicai-fe/fe-training/blob/master/webpack-chunk/development%26production/webpack.bundle.analyzer.js)
3. webpack插件耗时查看工具：`speed-measure-webpack-plugin` [案例](https://github.com/meicai-fe/fe-training/blob/master/webpack-chunk/development%26production/webpack.optimization.js)
4. 尝试使用 DllPlugin&DllReferencePlugin 后你会发现无法动态添加vendor.dll.js文件 而且无法添加hash值  这里推荐 `html-webpack-include-assets-plugin` [案例](https://github.com/weijiyang/supply-web/blob/master/config/webpack.prod.js)

### 相关知识拓展

#### webpack v3 v4区别有哪些呢？
v4 mode关键字有development、production两个选项  默认为production 同时如同上面提到的新增了默认打包规则 
development 模式下，默认开启了NamedChunksPlugin 和NamedModulesPlugin方便调试，提供了更完整的错误信息，更快的重新编译的速度。

production 模式下，由于提供了splitChunks和minimize，所以基本零配置，代码就会自动分割、压缩、优化，同时 webpack 也会自动帮你 Scope hoisting 和 Tree-shaking。

>Scope Hoisting 的实现原理其实很简单：分析出模块之间的依赖关系，尽可能的把打散的模块合并到一个函数中去，但前提是不能造成代码冗余。

>Tree-shaking的本质是消除无用的js代码。无用代码消除在广泛存在于传统的编程语言编译器中，编译器可以判断出某些代码根本不影响输出，然后消除这些代码

更多的v3 v4区别可以看这里： [手摸手，带你用合理的姿势使用webpack4](https://juejin.im/post/5b56909a518825195f499806
)

#### 相关链接
* package.json 中 scripts配置方法？[npm scripts 使用指南](http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html)
* Node & webpack 全局变量配置：[cross-env](https://www.npmjs.com/package/cross-env) 
* Node path.resolve path.join区别：[path的join和resolve](https://www.jianshu.com/p/78fadd20ee61
)


 (๑•̀ㅂ•́)و✧详细内容可查阅ppt [demo & ppt](https://github.com/meicai-fe/fe-training/tree/master/webpack-chunk) 或 [webpack chunk分享视频 by糖·少](https://pan.baidu.com/s/195e4bhhBj-0ES39rXfE6SQ 提取码: x5nc 复制这段内容后打开百度网盘手机App，操作更方便哦)

Thanks ！
