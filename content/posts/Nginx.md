+++
title = "Nginx使用"
date = 2026-01-03

[taxonomies]
tags = ["nginx"]
+++
<!-- more -->

## 第 1 章 Nginx 简介

### 1.1 什么是 Nginx

- Nginx 是一款轻量级的 Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器，在 BSD-like 协议下发行。其特点是占有内存少，并发能力强，事实上 nginx 的并发能力在同类型的网页服务器中表现较好，中国大陆使用 nginx 网站用户有：百度、京东、新浪、网易、腾讯、淘宝等。
- 英文主页：[http://nginx.net](http://nginx.net/)

### 1.2 Nginx 作为 web 服务器

- Nginx 可以作为静态页面的 web 服务器，同时还支持 CGI 协议的动态语言，比如 perl、php 等。但是不支持 java。Java 程序只能通过与 tomcat 配合完成。
- Nginx 专为性能优化而开发，性能是其最重要的考量，实现上非常注重效率，能经受高负载的考验，有报告表明能支持高达 50,000 个并发连接数。

### 1.3 正向代理

- 说反向代理之前，我们先看看正向代理，正向代理也是大家最常接触的到的代理模式，下面来解释一下什么叫正向代理：
- 在如今的网络环境下，我们如果由于技术需要要去访问国外的某些网站，此时你会发现位于国外的某网站我们通过浏览器是没有办法访问的，此时大家可能都会用一个操作 FQ 进行访问，FQ 的方式主要是找到一个可以访问国外网站的代理服务器，我们将请求发送给代理服务器，代理服务器去访问国外的网站，然后将访问到的数据传递给我们！（如图：）

![正向代理示意图](/img/nginx.png)

### 1.4 反向代理

- 反向代理，其实客户端对代理是无感知的，因为客户端不需要任何配置就可以访问，我们只需要将请求发送到反向代理服务器，由反向代理服务器去选择目标服务器获取数据后，在返回给客户端，此时反向代理服务器和目标服务器对外就是一个服务器，暴露的是代理服务器地址，隐藏了真实服务器 IP 地址。

![反向代理示意图](/img/nginx1.png)

**反向代理，"它代理的是服务端"，主要用于服务器集群分布式部署的情况下，反向代理隐藏了服务器的信息。**

- **反向代理的作用：**
    - 保证内网的安全，通常将反向代理作为公网访问地址，Web 服务器是内网
    - 负载均衡，通过反向代理服务器来优化网站的负载

### 1.5 负载均衡

- **客户端发送多个请求到服务器，服务器处理请求，有一些可能要与数据库进行交互，服务器处理完毕后，再将结果返回给客户端。**
- **这种架构模式对于早期的系统相对单一，并发请求相对较少的情况下是比较适合的，成本也低。但是随着信息数量的不断增长，访问量和数据量的飞速增长，以及系统业务的复杂度增加，这种架构会造成服务器相应客户端的请求日益缓慢，并发量特别大的时候，还容易造成服务器直接崩溃。很明显这是由于服务器性能的瓶颈造成的问题，那么如何解决这种情况呢？**
- **我们首先想到的可能是升级服务器的配置，比如提高 CPU 执行频率，加大内存等提高机器的物理性能来解决此问题，但是我们知道摩尔定律的日益失效，硬件的性能提升已经不能满足日益提升的需求了。最明显的一个例子，天猫双十一当天，某个热销商品的瞬时访问量是极其庞大的，那么类似上面的系统架构，将机器都增加到现有的顶级物理配置，都是不能够满足需求的。那么怎么办呢？**
- **上面的分析我们去掉了增加服务器物理配置来解决问题的办法，也就是说纵向解决问题的办法行不通了，那么横向增加服务器的数量呢？这时候集群的概念产生了，单个服务器解决不了，我们增加服务器的数量，然后将请求分发到各个服务器上，将原先请求集中到单个服务器上的情况改为将请求分发到多个服务器上，将负载分发到不同的服务器，也就是我们所说的负载均衡**

![负载均衡示意图](/img/nginx2.png)

### 1.6 动静分离

- 为了加快网站的解析速度，可以把动态页面和静态页面由不同的服务器来解析，加快解析速度。降低原来单个服务器的压力。

![动静分离示意图](/img/nginx3.png)

---

## 第 2 章 Nginx 安装

### 2.1 安装依赖包

```bash
yum -y install gcc zlib zlib-devel pcre-devel openssl openssl-devel
```

**一次性安装 4 个依赖**

### 2.2 下载并安装 Nginx

```bash
cd /opt     # 进入根目录下的 opt 目录
wget http://nginx.org/download/nginx-1.16.1.tar.gz     # 下载tar包
tar -zxvf nginx-1.16.1.tar.gz -C /usr/local/java       # 解压到 /usr/local/java 目录下（java目录需要自行创建）

# 解压完成后，你会在 /usr/lcoal/java 目录下会多出一个目录 nginx-1.16.1

cd /usr/local/java/nginx-1.16.1      # 进入 nginx-1.16.1 目录
./configure                          # 执行 ./configure 命令
make && make install                 # 编译并安装

# 编译安装完后，在 /usr/local/ 目录下会自动生成一个 nginx 目录，代表安装成功！
```

### 2.3 启动 Nginx

```bash
cd /usr/local/nginx/sbin/      # 进入 sbin 目录
./nginx                        # 启动 Nginx
```

### 2.4 防火墙放行端口

**Nginx 默认端口为 80**

- 防火墙这一块又涉及到一个知识点：
  - 在 CentOS 7.0 以上使用的是 **firewall**
  - CentOS 7.0 以下使用的是 **iptables**
- **具体操作请参考** [Linux防火墙firewall和iptables的使用](https://www.cnblogs.com/orangebooks/p/12045269.html)

### 2.5 访问验证

**访问 ip:80 （例如：192.168.32.41:80）**

**出现 `Welcome to nginx!` 表示成功**

---

## 第 3 章 Nginx 常用命令及配置文件

### 3.1 Nginx 常用命令

```bash
cd /usr/local/nginx/sbin      # 首先进入 sbin 目录
./nginx              # 启动 Nginx
./nginx -s stop      # 停止 Nginx
./nginx -s reload    # 重新加载 Nginx
./nginx -v           # 查看 Nginx 版本
```

**注意：一定要在 `/usr/local/nginx/sbin` 目录下执行命令才生效**

### 3.2 Nginx 的配置文件

**Nginx 的配置文件是在 `/usr/local/nginx/conf/` 目录中的 `nginx.conf` 文件**

**Nginx 的配置文件分由三部分组成**

**以下为 nginx 的默认配置文件**

```nginx
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```

---

#### 第一部分：全局块

从配置文件开始到 events 块之间的内容，主要会设置一些影响 nginx 服务器整体运行的配置指令，主要包括配置运行 Nginx 服务器的用户（组）、允许生成的 worker process 数，进程 PID 存放路径、日志存放路径和类型以及配置文件的引入等。

比如上面第一行的配置：

```nginx
worker_processes 1;
```

这是 Nginx 服务器并发处理服务的关键配置，worker_processes 值越大，可以支持的并发处理量也越多，但是会受到硬件、软件等设备的制约。

---

#### 第二部分：events 块

比如上面的配置：

```nginx
events {
    worker_connections  1024;
}
```

events 块涉及的指令主要影响 Nginx 服务器与用户的网络连接，常用的设置包括是否开启对多 work process 下的网络连接进行序列化，是否允许同时接收多个网络连接，选取哪种事件驱动模型来处理连接请求，每个 word process 可以同时支持的最大连接数等。

上述例子就表示每个 work process 支持的最大连接数为 1024。

这部分的配置对 Nginx 的性能影响较大，在实际中应该灵活配置。

---

#### 第三部分：http 块

这算是 Nginx 服务器配置中最频繁的部分，代理、缓存和日志定义等绝大多数功能和第三方模块的配置都在这里。

**需要注意的是：http 块也可以包括 http 全局块 和 server 块。**

---

**①、http 全局块**

http 全局块配置的指令包括文件引入、MIME-TYPE 定义、日志自定义、连接超时时间、单链接请求数上限等。

---

**②、server 块**

这块和虚拟主机有密切关系，虚拟主机从用户角度看，和一台独立的硬件主机是完全一样的，该技术的产生是为了节省互联网服务器硬件成本。

每个 http 块可以包括多个 server 块，而每个 server 块就相当于一个虚拟主机。

而每个 server 块也分为全局 server 块，以及可以同时包含多个 locaton 块。

**1、全局 server 块**

最常见的配置是本虚拟机主机的监听配置和本虚拟主机的名称或 IP 配置。

**2、location 块**

一个 server 块可以配置多个 location 块。

这块的主要作用是基于 Nginx 服务器接收到的请求字符串（例如 server_name/uri-string），对虚拟主机名称（也可以是 IP 别名）之外的字符串（例如 前面的 /uri-string）进行匹配，对特定的请求进行处理。地址定向、数据缓存和应答控制等功能，还有许多第三方模块的配置也在这里进行。

---

## 项目中配置

配置文件地址：`/usr/local/nginx/conf/`

文件名称：`treasurycloud-gateway.conf`

重新启动 nginx:

```bash
cd /usr/local/nginx/sbin/
./nginx -s reload
```

## Nginx 常用命令

查看版本，以及配置文件地址

```bash
nginx -V
```

查看版本

```bash
nginx -v
```

指定配置文件

```bash
nginx -c filename
```

帮助

```bash
nginx -h
```

重新加载配置 | 重启 | 停止 | 退出

```bash
nginx -s reload|reopen|stop|quit
```

打开 nginx

```bash
sudo nginx
```

测试配置是否有语法错误

```bash
sudo nginx -t
```

### 编辑 Nginx 配置文件

配置文件路径：`/usr/local/etc/nginx/nginx.conf`

找到 http 模块

![Nginx HTTP 模块配置](/img/nginx5.png)

---

## 参考资料

资料链接：[https://www.cnblogs.com/orangebooks/p/12058830.html](https://www.cnblogs.com/orangebooks/p/12058830.html)
