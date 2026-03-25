+++
title = "探究call 和 apply 的原理"
date = 2019-02-18

[taxonomies]
tags = ["web"]
+++
<!-- more -->
# 先拿call开刀
> 作用：call和apply都是替换函数内错误的this

```javascript
    var a = {
        value:1
    }
    var b = function(){
     

> 音乐🎵搭配学习更美味哦！
<img src="/img/callAndapply.jpg">
建议看这片文章时可以点击[音乐🎵](https://music.163.com/#/song?id=1293886117)，来个单曲循，美滋滋
   console.log(this.value) // 如果不对this进行绑定执行bar() 会返回undefined
    }
    b.call(a) //1
```
去除繁琐的讲解，一步到位自己模拟call的用法写一个函数，达到相同目的

```javascript
    Function.prototype.myCall = function(context){
        var context = context || window; //当没传入值时候，就是指全局window
        context.fn = this; //把调用myCall前的方法缓存下来
        var args = [...arguments].slice(1);//使用...打散传入值，并去除第一方法，得到一个数组
        var result = context.fn(...args);//把数组打散，把dinging 18传入b方法中
        delete context.fn; //删除
        return result
    }
    var a = {
        value:1
    }
    var b = function(name,age){
        console.log(this.value)
        console.log(name)
        console.log(age)
    }
    b.myCall(a,"dingding",18)
```
## apply
> apply的方法和 call 方法的实现类似，只不过是如果有参数，以数组形式进行传递

apply这个API平时使用的场景，代码如下:
```javascript
    var a = {
        value:1
    }
    var b = function(name,age){
        console.log(this.value)
        console.log(name)
        console.log(age)
    }
    b.apply(a,["dingding",18])
```
直接上模拟apply功能代码
```javascript
    Function.prototype.myApply = function(context){
        var context = context || window;
        context.fn = this;
        var result;
        if(arguments[1]){
            result = context.fn(...arguments[1]) 
        }else{
            result = context.fn()
        }
        delete context.fn
        return result
    }
    var a = {
        value:1
    }
    var b = function(name,age){
        console.log(this.value)
        console.log(name)
        console.log(age)
    }
    b.myApply(a,["dingding",18])
```
[参考资料](https://www.jianshu.com/p/92b48caee4b2)