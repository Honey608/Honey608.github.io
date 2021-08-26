---
title: js获取事件坐标位置
date: 2019-02-14 17:21:15
categories: web
tags: 
    - screenX/screenY
    - clientX/slientY
    - offsetX/offsetY
    # - onscroll/scrollTop/scrollTo/scrollBy
copyright:
keywords:
description:
---
{% cq %} 心有猛虎，细嗅蔷薇 {% endcq %}
<!--more-->
# 关于scrollTop,offsetTop,scrollLeft,offsetLeft用法介绍
```javascript
页可见区域宽： document.body.clientWidth;
网页可见区域高： document.body.clientHeight;
网页可见区域宽： document.body.offsetWidth (包括边线的宽);
网页可见区域高： document.body.offsetHeight (包括边线的宽);
网页正文全文宽： document.body.scrollWidth;
网页正文全文高： document.body.scrollHeight;
网页被卷去的高： document.body.scrollTop;
网页被卷去的左： document.body.scrollLeft;
网页正文部分上： window.screenTop;
网页正文部分左： window.screenLeft;
屏幕分辨率的高： window.screen.height;
屏幕分辨率的宽： window.screen.width;
屏幕可用工作区高度： window.screen.availHeight;
```
<img src="/img/location.jpeg">

# 事件
## 坐标事件
1.相对于显示屏左上角:e.screenX,e.screenY
2.相对于文档显示区左上角:e.clientX,e.clientY
3.相对于div左上角:e.offsetX,e.offsetY

---

## 页面滚动事件
window.onscroll  当页面滚动时触发
		      获取滚动距离：document.body.scrollTop | |  document.documentElement.scrollTop
      主动控制页面的滚动位置：
			window.scrollTo（横向滚动到的位置，纵向滚动到的位置）
			window.scrollBy(横向滚动的距离，纵向滚动的距离)

# 参考资料
[搞清clientHeight、offsetHeight、scrollHeight、offsetTop、scrollTop](https://blog.csdn.net/qq_35430000/article/details/80277587)
