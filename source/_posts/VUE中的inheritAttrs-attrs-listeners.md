---
title: 'VUE中的inheritAttrs,attrs,listeners'
date: 2020-05-26 11:15:22
categories: web
tags:
    — Vue中inheritAttrs，$attrs，$listeners
copyright:
keywords:
description:
---
{% cq %} Vue中的inheritAttrs，$attrs，$listeners使用总结 {% endcq %}
<!--more-->
# 使用场景
> 当你想向嵌套层级比较深组件数据传递，只能由父组件传递给子组件，子组件再传递给孙子组件,当传递十几个当情况，我们会在每个组件不停的props，每个必须写很多遍，除了vuex的父子组件通信，也可以通过inheritAttrs选项，以及实例属性$attrs

```javascript
      <template>
          <div class="home">
            <mytest  :title="title" :massgae="massgae"></mytest>
          </div>
        </template>
          export default {
            name: 'home',
            data () {
              return {
                title:'title1111',
                massgae:'message111'
              }
            },
            components:{
              'mytest':{
                template:`<div>这是个h1标题{{title}}</div>`,
                props:['title'],
                data(){
                  return{
                    mag:'111'
                  }
                },
                created:function(){
                  console.log(this.$attrs)//注意这里
                }
              }
            }
          }
```

  > 上边的代码，我们在组件里只是用了title这个属性，massgae属性我么是没有用的，那么下浏览器渲染出来是什么样呢？如下图：
  <img src="/img/7579449-9b4f591d793547b6.webp">
  > 我们看到：组件内未被注册的属性将作为普通html元素属性被渲染，如果想让属性能够向下传递，即使prop组件没有被使用，你也需要在组件上注册。这样做会使组件预期功能变得模糊不清，同时也难以维护组件的DRY。在Vue2.4.0,可以在组件定义中添加inheritAttrs：false，组件将不会把未被注册的props呈现为普通的HTML属性。但是在组件里我们可以通过其$attrs可以获取到没有使用的注册属性，如果需要，我们在这也可以往下继续传递。
  如果我们在子组件里设置 inheritAttrs: false：

  ```javascript
    components:{
    'mytest':{
      template:`<div>这是个h1标题{{title}}</div>`,
      props:['title'],
      inheritAttrs: false,
      data(){
        return{
          mag:'111'
        }
      },
      created:function(){
        console.log(this.$attrs)//注意这里
      }
    }
  ```
  <img src="/img/7579449-a8a0b3970380eeeb.webp">

  # $attrs的使用
  有一个页面由父组件，子组件，孙子组件构成，如下：
  ```javascript
    <template>
    <div style="padding:50px;">
        <childcom :name="name" :age="age" :sex="sex"></childcom>
    </div>
</template>
<script>
export default {
    'name':'test',
    props:[],
    data(){
        return {
            'name':'张三',
            'age':'30',
            'sex':'男'
        }
    },
    components:{
        'childcom':{
            template:`<div>
                <div>{{name}}</div>
                <grandcom v-bind="$attrs"></grandcom>
            </div>`,
            props:['name'],
            components: {
                'grandcom':{
                    template:`<div>{{$attrs}}</div>`,
                }
            }
        }
    }
}
</script>
  ```
上面的代码在页面的效果是如下图
<img src="/img/7579449-b2ab8dfe80fe828d.webp">
如果attrs被绑定在子组件childcom上后，我们就可以在孙子组件grandcom里获取到this.$attrs的值。这个{{$attrs}}的值是父组件中传递下来的props（除了子组件childcom组件中props声明的）。

记住孙子组件grandcom里获取到this.$attrs的值是除了子组件childcom声明的元素！记住是除了子组件childcom声明的元素！例如上面的代码我在子组件childcom组件的props里声明了name，那么我在孙子组件grandcom里获取到的$attrs就不包含name属性，那么this.$attrs = { 'age':'30', 'sex':'男'}。
# 说一下$attrs的优势到底在哪
假如我们要做一个页面，有父组件，子组件，孙子组件，如下：
```javascript
<template>
    <div>
        <childcom></childcom>
    </div>
</template>
<script>
export default {
    'name':'test',
    props:[],
    data(){
        return {
            'name':'张三',
            'age':'30',
            'sex':'男'
        }
    },
    components:{
        'childcom':{
            template:`<div>
                <div>我是子组件</div>
                <grandcom></grandcom>
            </div>`,
            components: {
                'grandcom':{
                    template:`<div>我是孙子组件</div>`,
                }
            }
        }
    }
}
</script>
```
如上代码，假如我想在子组件想获取到父组件的name属性值，在孙子组件获取父组件的age属性值，用props的话就必须在父组件把name和age的值通过props传递到子组件，子组件在通过props把age的值传递到孙子组件，到这里看明白了吧，孙子组件需要的age在子组件里没有用到，但是为了能让孙子组件获取到，你必须从父组件 传到子组件，在在子组件传递到孙子组件。

但是用$attrs就不用那么麻烦，如下：
```javascript
<template>
    <div>
        <childcom :name="name" :age="age" :sex="sex"></childcom>
    </div>
</template>
<script>
export default {
    'name':'test',
    props:[],
    data(){
        return {
            'name':'张三',
            'age':'30',
            'sex':'男'
        }
    },
    components:{
        'childcom':{
            props:['name'],
            template:`<div>
                <div>我是子组件   {{name}}</div>
                <grandcom v-bind="$attrs"></grandcom>
            </div>`,
            components: {
                'grandcom':{
                    template:`<div>我是孙子组件{{$attrs.age}}</div>`,
                }
            }
        }
    }
}
</script>
```
子组件绑定了"$attrs"，孙子组件就能获取到除了name属性外所有由父组件传递下来的属性。如果孙子组件也想获取到name属性那么，在绑定个name如下，
```javascript
 <grandcom v-bind="$attrs" :name="name"></grandcom>
```
细细体会下是不是这个道理

# 补充一下：inheritAttrs属性
关于inheritAttrs这个属性跟获取到$attrs的值没有关系，inheritAttrs通常在编写基础组件时候会用到。官网原话：默认情况下父作用域的不被认作 props 的特性绑定 (attribute bindings) 将会“回退”且作为普通的 HTML 特性应用在子组件的根元素上。当撰写包裹一个目标元素或另一个组件的组件时，这可能不会总是符合预期行为。通过设置 inheritAttrs 到 false，这些默认行为将会被去掉。而通过 (同样是 2.4 新增的) 实例属性 $attrs 可以让这些特性生效，且可以通过 v-bind 显性的绑定到非根元素上。

注意：这个选项不影响 class 和 style 绑定。

在Vue2.4.0之前版本，组件内未被注册的属性将作为普通html元素属性被渲染。

inheritAttrs到底有啥用？到底用在哪里？看下边代码，

```javascript
<template>
    <childcom :name="name" :age="age" type="text"></childcom>
</template>
<script>
export default {
    'name':'test',
    props:[],
    data(){
        return {
            'name':'张三',
            'age':'30',
            'sex':'男'
        }
    },
    components:{
        'childcom':{
            props:['name','age'],
            template:`<input type="number" style="border:1px solid blue">`,
        }
    }
}
</script>
```
上面代码你觉得input上会怎么显示？ 父组件传递了type="text"，子组件里input 上type="number"，那渲染到页面会是什么样？渲染图如下：
<img src="/img/inheritAttrs2.webp">
看到没，父组件传递的type="text"覆盖了input 上type="number"，这岂不是把我的input数据类型都给改变了，这岂不是有问题，这不是我想要的！！！！看到这里明白了吗？回头去体会下上面官网的原话！！！

需求：我需要input 上type="number"类型不变，但是我还是要取到父组件的type="text"的值，那么代码如下：
```javascript
<template>
    <childcom :name="name" :age="age" type="text"></childcom>
</template>
<script>
export default {
    'name':'test',
    props:[],
    data(){
        return {
            'name':'张三',
            'age':'30',
            'sex':'男'
        }
    },
    components:{
        'childcom':{
            inheritAttrs:false,
            props:['name','age'],
            template:`<input type="number" style="border:1px solid blue">`,
            created () {
                console.log(this.$attrs.type)
            }
        }
    }
}
</script>
```
页面渲染图如下：
<img src="/img/inheritAttrs1.webp">

到这，我想大家都明白了inheritAttrs的作用了吧。默认情况下vue会把父作用域的不被认作 props 的特性绑定 且作为普通的 HTML 特性应用在子组件的根元素上。绑定就绑定，显示就显示，没啥大不了的，但是怕就怕遇到一些特殊的，就比如上面的input的情况，这个时候inheritAttrs:false的作用就出来啦。

# 顺道补充一下：$listeners
父组件-子组件-孙子组件，，，，现在我要你在孙子组件里改变父组件的值，怎么改？有很多方法啦，但是$listeners给我们提供了一个新的思路。话不多说，直接上代码
```javascript
<template>
    <div>
        <childcom :name="name" :age="age" :sex="sex" @testChangeName="changeName"></childcom>
    </div>
</template>
<script>
export default {
    'name':'test',
    props:[],
    data(){
        return {
            'name':'张三',
            'age':'30',
            'sex':'男'
        }
    },
    components:{
        'childcom':{
            props:['name'],
            template:`<div>
                <div>我是子组件   {{name}}</div>
                <grandcom v-bind="$attrs" v-on="$listeners"></grandcom>
            </div>`,
           
            components: {
                'grandcom':{
                    template:`<div>我是孙子组件-------<button @click="grandChangeName">改变名字</button></div>`,
                    methods:{
                        grandChangeName(){
                           this.$emit('testChangeName','kkkkkk')
                        }
                    }
                }
            }
        }
    },
    methods:{
        changeName(val){
            this.name = val
        }
    }
}
</script>
```
页面渲染如下：
<img src="/img/inheritAttrs3.webp">
$listeners可以让你在孙子组件改变父组件的值，是不是很方便............


