---
title: websocket相关的webapi基本使用
date: 2020-12-06 13:15:27
tags:
    - websocket
categories:
    - webapi
descriptions:
---

### 为什么使用
 HTTP 协议有一个缺陷：通信只能由客户端发起。服务器返回查询结果。HTTP 协议做不到服务器主动向客户端推送信息。而使用WebSocket 服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等对话，属于服务器推送技术的一种。

### 特点
1. 建立在 TCP 协议之上，服务器端的实现比较容易。
2. 与 HTTP 协议有着良好的兼容性。默认端口也是80和443，并且握手阶段采用 HTTP 协议，因此握手时不容易屏蔽，能通过各种 HTTP 代理服务器。
3. 数据格式比较轻量，性能开销小，通信高效。
4. 可以发送文本，也可以发送二进制数据。
5. 没有同源限制，客户端可以与任意服务器通信。
6. 协议标识符是ws（如果加密，则为wss），服务器网址就是 URL。

### 客户端API

api地址：[WebSocket-MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket)

#### 创建
WebSocket 对象提供了用于创建和管理 WebSocket 连接，以及可以通过该连接发送和接收数据的 API。
``` js
var ws = new WebSocket("wss://echo.websocket.org");
```
#### 状态
websocket定义了四个常量

Constant|Value
---|---
WebSocket.CONNECTING|0
WebSocket.OPEN|1
WebSocket.CLOSING|2
WebSocket.CLOSED|3

> 对应了WebSocket.readyState的四个状态

```js
switch (ws.readyState) {//当前状态
  case WebSocket.CONNECTING:
    // do something
    break;
  case WebSocket.OPEN:
    // do something
    break;
  case WebSocket.CLOSING:
    // do something
    break;
  case WebSocket.CLOSED:
    // do something
    break;
  default:
    // this never happens
    break;
}
```
#### 事件
> 使用 addEventListener() 或将一个事件监听器赋值给本接口的 oneventname 属性，来监听下面的事件。

事件 | 用途
---|---
close|当一个 WebSocket 连接被关闭时触发。也可以通过 onclose 属性来设置。
error|当一个 WebSocket 连接因错误而关闭时触发，例如无法发送数据时。也可以通过 onerror 属性来设置.
message|当通过 WebSocket 收到数据时触发。也可以通过 onmessage 属性来设置。
open|当一个 WebSocket 连接成功时触发。也可以通过 onopen 属性来设置。
```js
ws.onopen = function(evt) { 
  console.log("Connection open ..."); 
  ws.send("Hello WebSockets!");
};

ws.onmessage = function(evt) {
  console.log( "Received Message: " + evt.data);
  ws.close();
};

ws.onclose = function(evt) {
  console.log("Connection closed.");
};
```
#### 方法


方法名 | 含义
---|---
WebSocket.close([code[, reason]])|关闭当前链接。
WebSocket.send(data)|对要传输的数据进行排队。

#### 属性


属性名 | 含义
---|---
WebSocket.binaryType|使用二进制的数据类型连接。
WebSocket.bufferedAmount 只读|未发送至服务器的字节数。
WebSocket.extensions 只读|服务器选择的扩展。
WebSocket.onclose|用于指定连接关闭后的回调函数。
WebSocket.onerror|用于指定连接失败后的回调函数。
WebSocket.onmessage|用于指定当从服务器接受到信息时的回调函数。
WebSocket.onopen|用于指定连接成功后的回调函数。
WebSocket.protocol 只读|服务器选择的下属协议。
WebSocket.readyState 只读|当前的链接状态。
WebSocket.url 只读|WebSocket 的绝对路径。

#### demo
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>websocket示例</title>
</head>

<body>
    <div>
        <button id="collect">发起连接</button>
        <button id="close">关闭连接</button>
        <input type="text" value="Hello WebSockets!" id="message">
        <button id="send">发送消息</button>
    </div>
    <script>
        var collect = document.getElementById('collect')
        var close = document.getElementById('close')
        var message = document.getElementById('message')
        var send = document.getElementById('send')
        var ws = null;
        collect.onclick = function() {
            //该地址是三方提供的websocket服务端地址，该服务接受客户端的信息并返回相同的信息
            var serverUrl = "wss://echo.websocket.org"
            ws = new WebSocket(serverUrl);
            ws.onopen = function(evt) {
                console.log("Connection open ...");
            };

            ws.onmessage = function(evt) {
                console.log("Received Message: " + evt.data);
            };

            ws.onclose = function(evt) {
                console.log("Connection closed.");
            };
        }
        close.onclick = function() {
            if (ws) ws.close()
        }
        send.onclick = function() {
            if (ws) ws.send(message.value);
        }
    </script>
</body>

</html>
```
### 服务端的实现

> 通过nodejs的插件实现:[nodejs-websocket](https://www.npmjs.com/package/nodejs-websocket)

#### 初始化安装
```bash
npm init
npm i nodejs-websocket
```
#### 创建
index.js
```js
var ws = require("nodejs-websocket")

// Scream server example: "hi" -> "HI!!!"
var server = ws.createServer(function(conn) {
    console.log("New connection")
    conn.on("text", function(str) {
        console.log("Received " + str)
        conn.sendText(str.toUpperCase() + "!!!")
    })
    conn.on("close", function(code, reason) {
        console.log("Connection closed")
    })
})
server.listen(9527, () => {
    console.log('服务启动了')
})
```
#### 启动
```bash
node index.js
```
> 此时客户端就可以发起连接到自己的服务器了。

### 扩展
[socket.io](https://socket.io/)