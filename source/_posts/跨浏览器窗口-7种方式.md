---
title: '跨浏览器窗口,7种方式'
date: 2021-09-01 08:26:58
categories: web
tags: ['跨浏览器窗口,7种方式']
copyright:
keywords:
description:
---
{% cq %} 跨浏览器窗口,7种方式 {% endcq %}

<!--more-->
#### WebSocket
    模拟聊天室
服务端代码
    ```javascript
        const WebSocket = require('ws');

        const server = new WebSocket.Server({
        port: 8080
        });

        server.on('open', function open() {
        console.log('connected');
        });

        server.on('close', function close() {
        console.log('disconnected');
        });

        server.on('connection', function connection(ws, req) {
        const ip = req.connection.remoteAddress;
        const port = req.connection.remotePort;
        const clientName = ip + port;

        console.log('%s is connected', clientName)

        // 发送欢迎信息给客户端
        ws.send("Welcome " + clientName);

        ws.on('message', function incoming(message) {
            console.log('received: %s from %s', message, clientName);

            // 广播消息给所有客户端
            server.clients.forEach(function each(client) {
            if (client.readyState === WebSocket.OPEN) {
                client.send(clientName + " -> " + message);
            }
            });

        });

        });
    ```
客户端的实现
    ```javascript
        <!DOCTYPE html>
        <html>
        <head>
            <meta charset="UTF-8">
            <title>WebSocket Chat</title>
        </head>
        <body>
            <script type="text/javascript">
                var socket;
                if (!window.WebSocket) {
                    window.WebSocket = window.MozWebSocket;
                }
                if (window.WebSocket) {
                    socket = new WebSocket("ws://localhost:8080/ws");
                    socket.onmessage = function (event) {
                        var ta = document.getElementById('responseText');
                        ta.value = ta.value + '\n' + event.data
                    };
                    socket.onopen = function (event) {
                        var ta = document.getElementById('responseText');
                        ta.value = "连接开启!";
                    };
                    socket.onclose = function (event) {
                        var ta = document.getElementById('responseText');
                        ta.value = ta.value + "连接被关闭";
                    };
                } else {
                    alert("你的浏览器不支持 WebSocket！");
                }

                function send(message) {
                    if (!window.WebSocket) {
                        return;
                    }
                    if (socket.readyState == WebSocket.OPEN) {
                        socket.send(message);
                    } else {
                        alert("连接没有开启.");
                    }
                }
            </script>
            <form onsubmit="return false;">
                <h3>WebSocket 聊天室：</h3>
                <textarea id="responseText" style="width: 500px; height: 300px;"></textarea>
                <br>
                <input type="text" name="message" style="width: 300px" value="您好">
                <input type="button" value="发送消息" onclick="send(this.form.message.value)">
                <input type="button" onclick="javascript:document.getElementById('responseText').value=''" value="清空聊天记录">
            </form>
        </body>
        </html>
    ```
#### 定时器 + 客户端存储

#### postMessage

#### StorageEvent
    page1
    ```javascript
        localStorage.setItem('message',JSON.stringify({
            message: '消息'，
            from: 'Page 1',
            date: Date.now()
        }))

    ```
    page2
    ```javascript
        localStorage.setItem('message',JSON.stringify({
            message: '消息'，
            from: 'Page 1',
            date: Date.now()
        }))
    ```
#### BroadcastChannel
    page1
    ```javascript
        var channel = new BroadcastChannel("channel-BroadcastChannel");
        channel.postMessage('Hello, BroadcastChannel!')
    ```
    page2
    ```javascript
        var channel = new BroadcastChannel("channel-BroadcastChannel");
        channel.addEventListener("message", function(ev) {
            console.log(ev.data)
        });
    ```
#### SharedWorker
> 我们可以在浏览器地址栏里面输入 `chrome://inspect，然后在侧边栏选中shared workers了，就可以看到浏览器，目前在运行的所有worker。点击inspect会打开一个开发者工具，然后就可以看到输出的log了
> 注意：如果要使 SharedWorker 连接到多个不同的页面，这些页面必须是同源的（相同的协议、host 以及端口）。
    worker.js
    ```javascript
        var portList = [];

        onconnect = function(e) {
            console.log('e', e)
            var port = e.ports[0];
            ensurePorts(port);
            port.onmessage = function(e) {
                var data = e.data;
                disptach(port, data);
            };
            port.start();
        };
        // 创建所有workder
        function ensurePorts(port) {
            if (portList.indexOf(port) < 0) {
                portList.push(port);
            }
        }
        // 发送其他页面
        function disptach(selfPort, data) {
            console.log('portList', portList)
            console.log('selfPort', selfPort)
            console.log('data', data)
            portList
                .filter(port => selfPort !== port)
                .forEach(port => port.postMessage(data));
        }
    ```
    page1
    ```javascript
        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8" />
            <meta name="viewport" content="width=device-width, initial-scale=1.0" />
            <meta http-equiv="X-UA-Compatible" content="ie=edge" />
            <title>BroadcastChannel Page 1</title>
        </head>
        <body>
            <h3>Page 1</h3>
            <section style="margin-top:50px; text-align: center">
            <input id="inputMessage" value="page 1的测试消息" />
            <input type="button" value="发送消息" id="btnSend" />
            <section id="messages">
                <p>收到的消息：</p>
            </section>
            </section>

            <script src="./worker.js"></script>
            <script>
            var messagesEle = document.getElementById("messages");
            var messageEl = document.getElementById("inputMessage");
            var btnSend = document.getElementById("btnSend");
            //

            if (!window.SharedWorker) {
                alert("浏览器不支持SharedWorkder!");
            } else {
                var myWorker = new SharedWorker("./worker.js");
                // 监听数据
                myWorker.port.onmessage = function(e) {
                console.log('page1', e)
                var msgEl = document.createElement("p");
                var data = e.data;
                msgEl.innerText = data.date + " " + data.from + ":" + data.message;
                messagesEle.appendChild(msgEl);
                };

                btnSend.addEventListener("click", function() {
                var message = messageEl.value;
                // 发送数据
                myWorker.port.postMessage({
                    date: new Date().toLocaleString(),
                    message,
                    from: "page 1"
                });
                });

                myWorker.port.start();
            }
            </script>
        </body>
        </html>
    ```
    page2
    ```javascript
        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8" />
            <meta name="viewport" content="width=device-width, initial-scale=1.0" />
            <meta http-equiv="X-UA-Compatible" content="ie=edge" />
            <title>BroadcastChannel Page 1</title>
        </head>
        <body>
            <h3>Page 2</h3>
            <section style="margin-top:50px; text-align: center">
            <input id="inputMessage" value="page 1的测试消息" />
            <input type="button" value="发送消息" id="btnSend" />
            <section id="messages">
                <p>收到的消息：</p>
            </section>
            </section>

            <script src="./worker.js"></script>
            <script>
            var messagesEle = document.getElementById("messages");
            var messageEl = document.getElementById("inputMessage");
            var btnSend = document.getElementById("btnSend");
            //

            if (!window.SharedWorker) {
                alert("浏览器不支持SharedWorkder!");
            } else {
                var myWorker = new SharedWorker("./worker.js");

                myWorker.port.onmessage = function(e) {
                console.log('page2', e)
                var msgEl = document.createElement("p");
                var data = e.data;
                msgEl.innerText = data.date + " " + data.from + ":" + data.message;
                messagesEle.appendChild(msgEl);
                };

                btnSend.addEventListener("click", function() {
                var message = messageEl.value;

                var message = messageEl.value;

                myWorker.port.postMessage({
                    date: new Date().toLocaleString(),
                    message,
                    from: "page 2"
                });
                });

                myWorker.port.start();
            }
            </script>
        </body>
        </html>
    ```
#### MessageChannel