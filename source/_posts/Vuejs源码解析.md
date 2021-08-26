---
title: Vuejs源码解析
date: 2019-01-31 16:40:13
categories:
tags:
copyright:
keywords:
description:
---
{% cq %} 除了使用vue工具，更想揭开神秘面纱,开始一场神秘之旅吧 {% endcq %}
<!--more-->
[vue.js源码地址](https://github.com/vuejs/vue/blob/dev/src/core/instance/index.js)
# 看后感觉内容不错的参考资料
1.[Vue技术内幕](http://hcysun.me/vue-design/art/2vue-constructor.html)
# 从一个简单的example开始
```javascript
    <div id="app">{{test}}</div>

    var vm = new Vue({
    el: '#app',
    data: {
            test: 1
        }
    })
    ```
这段代码简单调用了*Vue*，传递了两个选项 *el* 和 *data*,这段代码在页面呈现的DOM如下:
```javascript
    <div id="app">1<div>
```
## 接下来我们看看上面的例子到底发生了什么？
首先当我们使用Vue构造函数的时候，第一句执行的代码到底是什么，所以我们找到Vue的构造函数，Vue的构造函数在*core/instance/index.js*
    ```javascript
    function Vue (options) {
        if (process.env.NODE_ENV !== 'production' &&
            !(this instanceof Vue)
        ) {
            warn('Vue is a constructor and should be called with the `new` keyword')
        }
        this._init(options)
    }
    ```
上面的代码一目了然，当new Vue构造函数时，执行的第一句代码时this._init(options)方法，options参数内容是我们调用Vue构造函数传入的
    ```javascript
    options = {
        el:'#app',
        data:{
            test:1
        }
    }
    ```
既然如此我们就找到 *_init方法*，_init 方法在 src/core/instance/init.js 文件被添加到 Vue 的原型上，下面我们就看看 _init 做了什么。
_init 方法的一开始，是这两句代码：
```javascript
    const vm: Component = this
    // a uid
    vm._uid = uid++
```
首先声明了一个常量vm，并且在vm上添加了一个属性 *_uid*，uid的初始值是0每次实例化一个 Vue 实例之后，uid 的值都会 ++。

## 接下去的代码如下
```javascript
    let startTag, endTag
    /* istanbul ignore if */
    if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
      startTag = `vue-perf-start:${vm._uid}`
      endTag = `vue-perf-end:${vm._uid}`
      mark(startTag)
    }
```
首先声明了startTag, endTag这两个参数(parameter)，其中if括号中的意思是：在非生产环境下，并且config.performance和mark都为真，才执行里面的代码，其中 config.performance 来自于 core/config.js 文件，我们知道，Vue.config 同样引用了这个对象，在 Vue 的官方文档中可以看到如下内容：
**Vue 提供了全局配置 Vue.config.performance，我们通过将其设置为 true，即可开启性能追踪**
## 你可以追踪四个场景的性能
1、组件初始化(component init)
2、编译(compile)，将模板(template)编译成渲染函数
3、渲染(render)，其实就是渲染函数的性能，或者说渲染函数执行且生成虚拟DOM(vnode)的性能
4、打补丁(patch)，将虚拟DOM渲染为真实DOM的性能

