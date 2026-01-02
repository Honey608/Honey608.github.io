---
title: 在mac appstor下载软件提示内存不足问题
date: 2019-08-15 10:04:03
categories: 用机小技巧
tags:
copyright:
keywords:
description:
---
{% cq %} 日积月累 {% endcq %}
<!--more-->
time machine搞的鬼。你原来空间不足时，但time machine存储的是你的“过去”你只禁用掉time machine，再删除time machine时间点就可以了，另外可以运行电脑一两天，也有可能就好了。你在关于存储空间中显示的不是真实值，用df -h查看才是真实的。只要运行sudo tmutil listlocalsnapshots /查看有还存储了那些time machine，再用下面的命令删除掉就可以了。