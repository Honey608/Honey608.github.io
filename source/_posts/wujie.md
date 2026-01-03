---
layout: title
title: wujie常见的报错
date: 2026-01-03 15:33:07
categories: web
tags: ["wujie"]
---
{% cq %} wujie在项目中的报错 {% endcq %}

<!--more-->
## **请求资源报错**
请求报错为：`Access to fetch at *** from origin *** has been blocked by CORS policy: No 'Access-Control-Allow-Origin'`

**原因：** 子应用跨域或者请求子应用资源没有携带 cookie

**解决方案：**

1. 如果是跨域导致的错误，参考 [**前提**](https://wujie-micro.github.io/doc/guide/start.html#%E5%89%8D%E6%8F%90)
2. 如果是求资源没有携带 cookie（一般请求返回码是 302 跳转到登录页），需要通过自定义 [**fetch**](https://wujie-micro.github.io/doc/api/startApp.html#fetch) 将`fetch`的`credentials`设置为`include`，这样`cookie`才会携带上去

**警告**

当**`credentials`**设置为**`include`**时，服务端的**`Access-Control-Allow-Origin`**不能设置为**`*`**，原因[**详见**](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#credentialed_requests_and_wildcards)，服务端可以这样设置：

```jsx
ctx.set("Access-Control-Allow-Origin", ctx.headers.origin);
```

## **冒泡系列组件（比如下拉框）弹出位置不正确**
**原因：** 比如`element-plus`采用了`popper.js`2.0 版本，这个版本计算位置会递归元素一直计算到`window.visualViewport`，但是子应用的`dom`挂载在`shadowRoot`上，并没有`window.visualViewport`这部分滚动量，导致偏移计算失败

**解决方案：** 将子应用将`body`设置为`position: relative`即可

## 线上部署获取静态资源报错
1. 主应用通过代理访问子应用

```jsx
'/extern': {
    target: 'http://10.60.44.251:9010',
    headers: {
      Host: `localhost:${PORT}`
    },
    logLevel: 'debug'
  }
```

1. 子应用修改为相对路径访问静态资源

```jsx
publicPath: './'
```

2. 使用和主应用一样的路由模式

```jsx
createRouter({
    history: window.__POWERED_BY_WUJIE__ ? createWebHashHistory() : createWebHistory()
})
```

