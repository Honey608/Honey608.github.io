---
title: Vue中slot的使用（通俗易懂）
date: 2019-04-22 14:27:23
categories: web
tags: ["slot的使用"]
copyright:
keywords:
description:
---
{% cq %} slot {% endcq %}
<img src="/img/slot.jpeg" style="width:305px;heigth:277px">
好久没更新博客啦，今天我又回来啦！
<!--more-->
# 为什么会出现插槽
我们经常需要向一个组件传递内容，像这样：
    ```javascript
        <alert-box>
            Something bad happened
        </alert-box>
    ```
但是现实却是很残酷，可能会给你来个Error!Something bad happened
> 好啦，我们可以进入正题啦！😜
    ```javascript
    <div id="app">
       <childer-component :items="items">
           <h2>单个插槽</h2>
           <span>只有在子组件使用单个插槽 slot 才能让我显示出来</span>

           <h2>多个插槽也叫具名插槽</h2>
           <div slot="one">
                <span>one</span>
                <span>第一个</span>
           </div>
           <div slot="two">
                <span>two</span>
                <span>第二个</span>
           </div>
           <h2>作用域插槽（将子组件的值传到父组件供使用）</h2>
           <div slot-scope="props">
                <span>{{ props.addr }}</span>
                <span>{{ props.cname }}</span>
                <span>{{ props.age }}</span>
           </div>
       </childer-component>
    </div>
    <script>
        var childerComponent = Vue.component('childer-component',{
            props:['items'],
            template:`
                <div>
                    <h1>我是子组件</h1>    
                    <slot>默认</slot> 

                    <slot name="one"></slot>
                    <slot name="two"></slot>  

                    <slot :cname="items[2].cname"></slot> 
                    <slot :addr="items[2].addr"></slot> 
                    <slot age="18"></slot> 
                </div>
            `
        })
        var vm = new Vue({
            el: '#app',
            data: {
                items:[
                    { text:'文字1' , cname:'tom' , addr:'usa' },
                    { text:'文字2' , cname:'wangwu' , addr:'uk' },
                    { text:'文字3' , cname:'zhangsan' , addr:'un' }
                ],
                components:{
                    'childerComponent':childerComponent,
                }
            }
        })
    </script>
    ```
