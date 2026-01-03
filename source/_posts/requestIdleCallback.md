---
layout: title
title: requestIdleCallback
date: 2026-01-03 16:03:39
categories: web
tags: ["requestIdleCallback"]
---
{% cq %} requestIdleCallback作用 {% endcq %}

<!--more-->
在网页中，有许多耗时但是却又不能那么紧要的任务。它们和紧要的任务，比如对用户的输入作出及时响应的之类的任务，它们共享事件队列。如果两者发生冲突，用户体验会很糟糕。我们可以使用setTimout，对这些任务进行延迟处理。但是我们并不知道，setTimeout在执行回调时，是否是浏览器空闲的时候。

而requestIdleCallback就解决了这个痛点，requestIdleCallback会在帧结束时并且有空闲时间。或者用户不与网页交互时，执行回调。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <input type="text" id="text" style="width: 700px" />
</body>
<script>
    const datas = []
    const text = document.getElementById('text')
    let isReporting = false


    function sleep (ms = 100) {
        let sleepSwitch = true
        let s = Date.now()
        while (sleepSwitch) {
            if (Date.now() - s > ms) {
                sleepSwitch = false
            }
        } 
    }
    function handleClick () {
        datas.push({
            date: Date.now()
        })
        // 监听用户响应的函数，需要花费150ms
        sleep(150)
        handleDataReport()
    }


    // =========================  使用requestIdleCallback  ==============================


    function handleDataReport () {
        if (isReporting) {
            return
        }
        isReporting = true
        requestIdleCallback(report)
    }


    function report (deadline) {
        isReporting = false
        while (deadline.timeRemaining() > 0 && datas.length > 0) {
            get(datas.pop())
        }
        if (datas.length) {
            handleDataReport()
        }
    }


    // =========================  使用requestIdleCallback结束  ==============================


    function get(data) {
        // 数据上报的函数，需要话费20ms
        sleep(20)
        console.log(`~~~ 数据上报 ~~~: ${data.date}`)
    }


    text.oninput = handleClick
</script>
</html>
```

参考资料：[https://juejin.cn/post/6844904081463443463](https://juejin.cn/post/6844904081463443463)

