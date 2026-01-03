---
layout: title
title: HTTP协议
date: 2026-01-02 21:00:57
categories: java
tags: ["HTTP协议"]
---
{% cq %} HTTP协议 {% endcq %}

<!--more-->

## 概念
超文本传输协议，规定了浏览器和服务器数据传输的规则。

<!-- 这是一张图片，ocr 内容为：请求 HTTP协议 响应数据 请求数据 响应 -->
![](/img/httpimage.png)

## 特点
1. 基于TCP协议：面向连接，安全
2. 基于请求-响应模型的：一次请求对应一次响应
3. HTTP协议是无状态的协议：对于事务处理没有记忆能力。每次请求-响应都是独立的。
+ 缺点：多次请求间不能共享数据。
+ 优点：速度快

## 请求数据格式
<!-- 这是一张图片，ocr 内容为：请求的主机名 HOST [浏览器版本,例如CHROME测览器的标识类似MOZILA/5.0................................... (WINDOWS USER-AGENT NT...)LIKE GECKO 表示浏览器能接收的资源类型,如TEXT/*,IMAGE/*或者*/*表示所有; ACCEPT 表示浏览器偏好的语言,服务器可以据此返回不同语言的网页; ACCEPT-LANGUAGE 表示浏览器可以支持的压缩类型,例如GZIP,DEFLATE等. ACCEPT-ENCODING CONTENT-TYPE 请求主体的数据类型. 请求主体的大小(单位:字节). CONTENT-LENGTH -->
![](/img/imageHttp3.png)

## 响应数据格式
### 常见状态码
<!-- 这是一张图片，ocr 内容为：状态 英文描述 解释 码 客户端请求成功,即处理成功,这是我们最想看到的状态码 OK 200 指示所请求的资源已移动到由LOCATION响应头给定的URL.浏览器会自动重新访问到这个页面 302 FOUND 告诉客户端,你请求的资源至上次取得后,服务瑞并未更改,你直接用你本地缓存吧.隐式重定向 NOT MODIFIED 304 客户端请求有语法错误,不能被服务器所理解 BAD REQUEST 400 服务器收到请求,但是拒绝提供服务,比如:没有权限访问相关资源 403 FORBIDDEN 请求资源不存在,一般是URL输入有误,或者网站资源被删除了 404 NOT FOUND 请求方式有误,比如应该用GET请求方式的资源,用了POST METHOD NOT ALLOWED 405 服务器要求有条件的请求,告诉客户需要想访问该资源,必须携带特定的请求头 PRECONDITION REQUIRED 428 指示用户在给定时向内发送了太多请求(限速),配合REUYAFERT多长时向后可以请求)响应头一起使用 TOO MANY REQUESTS 429 请来头太大,服务器不感意处理请求,因为它的头部字段太大,请求可以在减少请求头域的大小后重新提 REQUEST HEADER FIELDS TOO 431 交. LARGE 服务器发生不可预期的错误.服务器出异常了,赶紧看日志去吧 INTERNAL SERVER ERROR 500 服务器尚未准备好处理请求,服务器刚刚启动,还未初始化好 SERVICE E UNAVAILABLE 503 -->
![](/img/imageHttp4.png)

### 响应格式
<!-- 这是一张图片，ocr 内容为：表示该响应内容的类型,例如TEXT/HTML,APPLICATION/JSON. CONTENT-TYPE 表示该响应内容的长度(字节数). CONTENT-LENGTH 表示该响应压缩算法,例如GZIP. CONTENT-ENCODING 指示客户端应如何缓存,例如MAX-AGE-300表示可以最多缓存300秒. CACHE-CONTROL 告诉浏览器为当前页面所在的域设置COOKIE. SET-COOKIE -->
![](/img/imageHttp2.png)

