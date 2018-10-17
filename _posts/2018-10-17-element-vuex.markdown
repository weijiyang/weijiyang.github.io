---
layout: post
title: Element&VueX同时使用遇到 计算值属性更改问题？？？（菜单与Tab联动）
date: 2018-10-17
description: error:Computed property "route" was assigned to but it has no setter
tags: [Vue, VueX, Element]
---

# 问题描述：Computed property "route" was assigned to but it has no setter

```
template：
<el-tabs type="card" v-model="route">
</el-tabs>
```

```
js：
computed: {
  route () {
    return this.$store.state.curTab.route
  }
}
```

* 问题出现的原因在于我们在使用Element菜单与Tab联动的时候、使用了computed或者是mapGetters 当我们使用dispatch调用actions mutations对state变量更改时   使用场景中的v-model也随之更改 这时就会出现上面的错误。
* 下面是解决方法：

```
js：
computed: {
  route: {
    get: function () {
      return this.$store.state.curTab.route
    },
    set: function () {
    }
  }
}
```