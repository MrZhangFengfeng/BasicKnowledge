## 基本状态码
- `1xx`   信息性状态码：服务器正在处理请求
- `2XX`   成功状态码：请求已正常处理完毕
- `3XX`   重定向状态码：需要进行额外操作以完成请求
- `4XX`   客户端错误状态码：户端原因导致服务器无法处理请求
- `5XX`   服务器错误状态码：服务器原因导致处理请求出错

- - -
## 常见状态码
- 100 Continue：初始的请求已经接受，客户应当继续发送请求的其余部分
- 101 Switching Protocols：服务器将遵从客户的请求转换到另外一种协议
- - -

- 200 OK：一切正常，对GET和POST请求的应答文档跟在后面
- 204 No Content：没有新文档，浏览器应该继续显示原来的文档。一般用在只是客户端向服务器发送信息，而服务器不用向客户端返回什么信息的情况
- 206 Partial Content： 客户发送了一个带有Range头的GET请求，服务器完成了它
- - -

- 300 Multiple Choices：客户请求的文档可以在多个位置找到，这些位置已经在返回的文档内列出。如果服务器要提出优先选择，则应该在Location应答头指明。
- 301 Moved Permanently：永久性重定向。
- 302 Found：临时性重定向。
- 303 See Other：表示请求资源存在另一个URI，应使用GET定向获取请求资源 。303功能与302一样，区别只是303明确客户端应该使用GET访问 
- 304 Not Modified：客户端有缓冲的文档并发出了一个条件性的请求（一般是提供If-Modified-Since头表示客户只想比指定日期更新的文档）。
服务器告诉客户，原来缓冲的文档还可以继续使用。
- - -

- 400 Bad Request：请求出现语法或者参数错误，服务器不理解 。
- 401 Unauthorized：客户试图未经授权访问受密码保护的页面。应答中会包含一个WWW-Authenticate头，浏览器据此显示用户名字/密码对话框，
然后在填写合适的Authorization头后再次发出请求。
- 403 Forbidden：资源不可用，被服务器拒绝了，大多是没有权限访问。
- 404 Not Found：无法找到指定位置的资源，也有可能服务器就是不想给你然后骗你找不到
- 405 Method Not Allowed：请求方法（GET、POST、HEAD、Delete、PUT、TRACE等）对指定的资源不适用。
- - -

- 500 Internal Server Error：服务器遇到了意料不到的情况，不能完成客户的请求
- 501 Not Implemented：服务器不支持实现请求所需要的功能。例如，客户发出了一个服务器不支持的PUT请求
- 503 Service Unavailable： 服务器由于维护或者负载过重未能应答。
- 504 Gateway Timeout：表示不能及时地从远程服务器获得应答
- 505 HTTP Version Not Supported：服务器不支持请求中所指明的HTTP版本


- - -
# OVER
