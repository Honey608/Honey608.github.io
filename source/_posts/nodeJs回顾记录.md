---
title: nodeJs回顾记录
date: 2019-11-25 14:11:50
categories: web
tags:
    - nodejs
copyright: true
keywords:
description:
---
{% cq %} Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行时 {% endcq %}
<img style="height: 176px;width: 50%;" src="/img/libuv.png">
<!--more-->

1. **在node_modules下的文件直接使用 require('nav') 使用** 
```
    node_modules下创建nav文件夹再创建nav.js
    //nav.js *** 主要使用nmp init -y 初始化 package.json文件中 "main": "nav.js" ***

        const str = 'hello world come from nav.js'
        module.exports = str

    //其他js文件
        const nav = require('nav')
        console.log(nav)  //hello world come from nav.js
```

2. ***常用的fs模块API使用***
```
    const fs = require('fs')

    // 1. 异步读取文件
    fs.readFile('./demo.txt',function(err,fd){
        if(err){
            return console.error(err)
        }
        console.log(fd.toString())
    })

    // 2. 打开文件夹
    fs.open('./demo.txt',function(err,fd){
        if(err){
            return console.error(err)
        }
        console.log(fd)
    })

    // 3. 获取文件信息
    fs.stat('./demo.txt',function(err,fd){
        if(err){
            return console.error(err)
        }
        console.log(fd.isFile())
    })

    // 4. 写入文件
    fs.writeFile('log.js', '写入日志3', function (err) {
        if (err) {
            return console.error(err)
        }
    })

    // 5. 写入内容
    fs.appendFile('./log.js','使用appendFile写入内容\n',function(err){
        if (err) {
            return console.error(err)
        }
        console.log('写入成功')
    })

    // 6. 读取目录 把目录下面的文件和文件夹都获取到
    fs.readdir('./src',function(err,fl){
        if (err) {
            return console.error(err)
        }
        console.log(fl)
    })

    // 7. fs.rename 1.重命名 2.剪切
    fs.rename('./src/rename.js','./src/rename1.js',function(err,res){
        if (err) {
            return console.error(err)
        }
        console.log('重命名成功')
    })
    fs.rename('./src/index.html','./static/index.js',function(err,re){
        if (err) {
            return console.error(err)
        }
        console.log('剪切成功')
    })

    // 8. fs.rmdir 删除目录
    fs.rmdir('index.html', function (err, re) {
        if (err) {
            return console.error(err)
        }
        console.log('删除目录成功')
    })

    //9. fs.unlink 删除文件
    fs.unlink('index.html', function (err, re) {
        if (err) {
            return console.error(err)
        }
        console.log('删除文件成功')
    })
```

2.1 *** 练习（打印src下是目录的文件)***
```
    //因为是异步操作，所以使用递归加匿名函数自调解决
    fs.readdir('src',function(err,files){
    var allFile = []
    if(err){
        return console.error(err)
    }else{
        (function a(i){
            if(files.length == i){
                console.log(allFile)
                return false
            }
            fs.stat('src/'+files[i],function(err,fd){
                if(fd.isDirectory()){
                    allFile.push(files[i])
                }
                a(i+1)
            })
        })(0)
    }
})
```

3. *** 读入流、写入流和管道读取 ***
```
    const fs = require('fs');

    //文件读取流（就是一段段读取）
    var readStream = fs.createReadStream('demo.txt');
    var str = '';
    var count = 0;
    readStream.on('data',function(chunk){
        str += chunk;
        count++;
    })
    readStream.on('end',function(chunk){
        console.log(count)
        console.log(str)
    })

    //文件写入流
    var writeStream = fs.createWriteStream('input.txt');
    var data = '使用createWriteStream流写入文件\n'
    //也可以使用for写入
    for(var i=0;i<90;i++){
        writeStream.write(data,'utf-8');
    }
    writeStream.end(); //标记写入完成 能触发以下方法
    // 成功
    writeStream.on('finish',function(){
        console.log('写入完成')
    })
    //失败
    writeStream.on('error',function(){
        console.log('写入完成')
    })

    //读取一个文件内容写入到另一个文件中
    var readerStream = fs.createReadStream('demo.txt'); //读取文件
    var writeStream = fs.createWriteStream('input.txt'); //创建一个可写入流
    readerStream.pipe(writeStream)
    console.log('程序执行完毕')

```

4. *** 写一个简单的web服务器，可以根据输入同url返回相应的文件内容 ***
```
    const http = require('http');
    const fs = require('fs');
    const path = require('path');
    const mime = require('./static/model/mime');

    http.createServer(function (req, res) {
        var pathname = req.url;
        pathname == '/' ? pathname = './index.html' : "";
        //获取文件的后缀名
        let exname = path.extname(pathname)
        if (pathname != '/favicon.ico') {
            fs.readFile('static/' + pathname, function (err, fd) {
                if (err) {
                    return console.error(err)
                } else {
                    var mimeName = mime.getMime(exname) //使用自己写的方法返回text-html,text-css等
                    res.writeHead(200, {
                        "Content-Type": "mimeName;chart=utf-8"
                    })
                    res.end(fd)
                }
            })
        }
    }).listen(8888)
```
4.1 *** 使用回调函数解决异步问题 ***
```
    const fs = require('fs');
    console.log(1)
    function getMime(callback){
        fs.readFile('input.txt',function(err,fd){
            if(err){
                return console.error(err)
            }
            callback(fd)
        })
    }
    getMime(function(data){
        console.log(data.toString())
    })
```
4.2 *** 使用nodejs自带的events方法解决异步问题 ***
```
    const fs = require('fs');
    const events = require('events');

    var EventEmitter = new events.EventEmitter();

    fs.readFile('input.txt', function (err, fd) {
        if (err) {
            return console.error(err)
        }
        EventEmitter.emit('to_parent', fd)
    })

    EventEmitter.on('to_parent', function (data) {
        console.log(data.toString())
    })
```
