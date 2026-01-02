---
title: Promise的使用
date: 2019-05-31 13:23:51
categories: web
tags:
    - Promise的使用
copyright:
keywords:
description:
---
{% cq %} 温故而知新，可以为师矣 {% endcq %}
<!--more-->
# ES6 Promise 用法讲解

Promise是一个构造函数，自己身上有all、reject、resolve这几个眼熟的方法，原型上有then、catch等同样很眼熟的方法。
## resolve参数（可以理解合格的内容）
    ```javascript
        <script>
            var p = new Promise(function(resolve,reject){
                setTimeout(() => {
                    console.log('执行完成');
                    resolve("合格的内容")
                },2000)
            })
        </script>
        先打印出了Promise {<pending>}
                    __proto__: Promise
                    [[PromiseStatus]]: "resolved"
                    [[PromiseValue]]: "合格的内容"
        再过2秒，打印出了'执行完成'
    ```
## 使用.then就可以获取resovle中的内容啦
    ```javascript
        <script>
             var p = new Promise(function(resolve,reject){
                setTimeout(() => {
                    console.log('执行完成');
                    resolve("合格的内容")
                },2000)
            })
            p.then( (val) => {
                console.log(val)
            })
        </script>
        2秒钟后就相继输出了 "执行完成" "合格的内容"
    ```
## resolve参数（可以理解不合格的内容）可以使用.catch获取它的内容
    ```javascript
        function getNumber(){
            var p = new Promise(function(resolve, reject){
                //做一些异步操作
                setTimeout(function(){
                    var num = Math.ceil(Math.random()*10); //生成1-10的随机数
                    if(num<=5){
                        resolve(num);
                    }
                    else{
                        reject('数字太大了');
                    }
                }, 2000);
            });
            return p;            
        }
        getNumber().then( val => {
            console.log(val)
        }).catch( val => {
            console.log(val)
        })
        如何满足if条件，就输出相应数字，否则输出 reject中的'数字太大了'
    ```
## all的用法
> 方法都调用完成再输出resolve中的内容
    ```javascript
        function runAsync1(){
        var p = new Promise(function(resolve, reject){
            //做一些异步操作
            setTimeout(function(){
                console.log('异步任务1执行完成');
                resolve('随便什么数据1');
            }, 1000);
        });
        return p;            
        }
        function runAsync2(){
            var p = new Promise(function(resolve, reject){
                //做一些异步操作
                setTimeout(function(){
                    console.log('异步任务2执行完成');
                    resolve('随便什么数据2');
                }, 2000);
            });
            return p;            
        }
        function runAsync3(){
            var p = new Promise(function(resolve, reject){
                //做一些异步操作
                setTimeout(function(){
                    console.log('异步任务3执行完成');
                    resolve('随便什么数据3');
                }, 300);
            });
            return p;            
        }
        Promise
        .all([runAsync1(),runAsync2(),runAsync3()])  //时间短的先执行
        .then(function(results){
            console.log(results);  //返回值按照数据中方法顺序输出
        });
        // 异步任务3执行完成 异步任务1执行完成 异步任务2执行完成
        // ["随便什么数据1", "随便什么数据2", "随便什么数据3"]
    ```
## race的用法
> 异步执行最快的函数以后，直接输出内容，再按照执行速度依次执行
    ```javascript
        function runAsync1(){
            var p = new Promise(function(resolve, reject){
                //做一些异步操作
                setTimeout(function(){
                    console.log('异步任务1执行完成');
                    resolve('随便什么数据1');
                }, 3000);
            });
            return p;            
            }
        function runAsync2(){
            var p = new Promise(function(resolve, reject){
                //做一些异步操作
                setTimeout(function(){
                    console.log('异步任务2执行完成');
                    resolve('随便什么数据2');
                }, 3000);
            });
            return p;            
        }
        function runAsync3(){
            var p = new Promise(function(resolve, reject){
                //做一些异步操作
                setTimeout(function(){
                    console.log('异步任务3执行完成');
                    resolve('随便什么数据3');
                }, 1000);
            });
            return p;            
        }
        Promise
        .race([runAsync1(), runAsync2(), runAsync3()])
        .then(function(results){
            console.log(results);
        });
        //异步任务3执行完成 随便什么数据3 异步任务1执行完成 异步任务2执行完成
    ```

