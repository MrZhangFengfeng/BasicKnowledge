# WebSocket

## 为什么我们有了HTTP协议后还需要websocket协议？

因为 HTTP 协议有一个缺陷：通信只能由客户端发起。HTTP 协议做不到服务器主动向客户端推送信息。这种单向请求的特点，注定了如果服务器有连续的状态变化，客户端要获知就非常麻烦。我们只能使用"轮询"：每隔一段时候，就发出一个询问，了解服务器有没有新的信息。

## 简介
WebSocket协议，服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等对话，属于服务器推送技术的一种。

还有一些其他特点：

（1）建立在 TCP 协议之上，服务器端的实现比较容易。

（2）与 HTTP 协议有着良好的兼容性。默认端口也是80和443，并且握手阶段采用 HTTP 协议，因此握手时不容易屏蔽，能通过各种 HTTP 代理服务器。

（3）数据格式比较轻量，性能开销小，通信高效。

（4）可以发送文本，也可以发送二进制数据。

（5）没有同源限制，客户端可以与任意服务器通信。

（6）协议标识符是ws（如果加密，则为wss,类似http==>https），服务器网址就是 URL。

> `ws://example.com:80/some/path`

## 客户端简单示例

    var ws = new WebSocket("wss://echo.websocket.org");

    ws.onopen = function(evt) { 
    console.log("Connection open ..."); 
    ws.send("链接开始，Hello WebSockets!");
    };

    ws.onmessage = function(evt) {
    console.log( "收到消息了，Received Message: " + evt.data);
    ws.close();
    };

    ws.onclose = function(evt) {
    console.log("链接结束，Connection closed.");
    };  

## 客户端语法

详情见 [阮一峰博客](http://www.ruanyifeng.com/blog/2017/05/websocket.html)。  
