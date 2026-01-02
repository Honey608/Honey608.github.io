---
title: 'webpack4.29.0基本用法'
date: 2019-01-25 17:11:32
categories: web
tags:
    - webpack4+
copyright: true
keywords:
description:
---

{% cq %} 不积跬步无以至千里，不积小流无以成江海 {% endcq %}
<img src="/img/jy.png">
<!--more-->
# 准备工作
+ 全局安装webpack和webpack-cli
    >sudo npm install webpack -g
    >sudo npm install webpack-cli -g

+ 文件夹下局部安装
    >npm init -y
    >sudo npm install webpack --save-dev
    >sudo npm install webpack-cli --save-dev

## 实现对一个js文件打包
``` 
    //webpack.config.js
    const path = require('path')
    module.exports = {
        mode:'development',
        entry:'./app.js',
        output:{
            filename:'[name].bundle.js',
            path:path.join(__dirname,'./dist'),
      }
    }
    这样就能在dist目录下出现一个app.bundle.js文件啦！
```
## 实现多个js文件打包
```
    //webpack.config.js
    const path = require('path')
    const CleanWebpackPlugin = require('clean-webpack-plugin')
    module.exports = {
        mode: 'development',
        entry: {
            index: './src/index.js',
            test: './src/test.js',
            test1: './src/test1.js',
        },
        output: {
            path: path.join(__dirname,'./dist/js'),
            filename:'[name]-[hash].js',
            pubicPath: 'http://cdn.con' //请求时会自己加协议（location.protocol='http'）和端口号(host='cdn.con')
        }，
        plugins: [
            new CleanWebpackPlugin (['./dist/js']) //清除之前打包的文件
        ]
    }
```
## 使用模版实现每个js对应自己的html
```
    //webpack.config.js
    const path = require('path')
    const HtmlWebpackPlugin = require('html-webpack-plugin') //模版loader
    const CleanWebpackPlugin = require('clean-webpack-plugin')
    module.exports = {
        entry:{
            index:'./src/index.js',
            test:'./src/test.js',
            test1:'./src/test1.js',
        },
        output:{
            path:path.join(__dirname,'./dist/js'),
            filename:'[name]-[hash].js',
            publicPath:"http://cdn.com"
        },
        plugins:[
            new HtmlWebpackPlugin({
                title: 'this a index.html', //每个html 的title
                template:'hello.html',
                filename:'index.html',
                excludeChunks:['test','test1'] //排除名为test.js,test1.js打包的js文件
            }),
            new HtmlWebpackPlugin({
                title:'this a test.js',
                template:'hello.html',
                filename:'test.html',
                excludeChunks:['index','test1']
            }),
            new HtmlWebpackPlugin({
                title:'this a test1.js',
                template:'hello.html',
                filename:'test1.html',
                excludeChunks:['test','index']       
            }),
            new CleanWebpackPlugin(['./dist/js'])
        ]
    }

    //hello.html 模版内容
    <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title><%= htmlWebpackPlugin.options.title%></title>
    <script type="text/javascript">
        <%=
        compilation.assets[htmlWebpackPlugin.files.chunks.test1.entry.substr
        (htmlWebpackPlugin.files.publicPath.length)].source() %>
    </script>
    </head>
    <body>
        <% for(let k in htmlWebpackPlugin.files.chunks) {%>
            <% if(k != 'index') {%>
                <script src="<%=htmlWebpackPlugin.files.chunks[k].entry %>"></script>
            <% }%>
        <% } %>
    </body>
```
## 使用file-loader|css-loader|style-loader|postcss-loader
```
    //webpack.config.js
    module.exports = {
    mode: 'development',
    entry: './app.js',
    output: {
        filename: '[name].bundle.js',
        path: path.join(__dirname, './dist/js1'),
    },
    plugins: [
        new CleanWebpackPlugin(['./dist/js1']),
        require('autoprefixer')
    ],
    module: {
        rules: [
        {
            test: /\.(le|c)ss$/,
            test: /\.(png|jpg|gif)$/,
            use: [
            { loader: "style-loader" },
            { loader: "css-loader" },
            { loader: 'file-loader'},
            {
                loader: "postcss-loader",
                options: {
                plugins: [
                    require("autoprefixer") /*在这里添加*/,
                ],
                }
            }
            ]

        }
        ]
     },
    }
```
### 简单打包一个库的演示

```
    //webapck.config.js
    const path = require('path');

    module.exports = {
        mode:'development',
        entry: './src/index.js',
        // 不打包进自己写的库的代码中，在业务代码中加载
        externals:['lodash'],
        output: {
            path: path.resolve(__dirname, 'dist'),
            // 文件打包后的文件名字
            filename: 'library.js',
            //可以使用src引入变为全局变量
            library:'library',   
            //以上library挂载的地方（比如this window等）
            libraryTarget: "umd"
        }
    };
```