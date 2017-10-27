## 手写一个ajax，不依赖第三方库

      var xhr = new XMLHttpReauest();
      xhr.open('get','/api',false);
      xhr.onreadystatechange = function() {
        if(xhr.readyState == 4) {
          if(xhr.state == 200) {
            console.log(xhr.responseText);
          }
        }
      }
      xhr.send(null);

附录：readState
- 0：未初始化，还没有调用send方法
- 1：载入，已调用send方法，正在发送请求
- 2：载入完成，send方法执行完成，已经接收到全部响应内容
- 3：交互，正在解析响应内容
- 4：完成，响应内容解析完成，可以在客户端调用了

## 可以跨域的三个标签
    <img src="...">
    <script src="...">
    <link href="...">

- img标签可用于打点统计，统计网站可能是其他域(百度统计)
- link  script 标签可以使用CDN，CDN的也是其他域
- script 用于JSONP

- - -
## JSPNP实现原理
- 加载 `http://baidu.com/demo.html`
- 服务器不一定就有这个HTML文件
- 服务器可以根据请求动态生成一个对应的文件，并返回
- 例如你要访问百度一个接口，百度给你一个地址：`http://news.baidu.com/index.js`
- 返回内容格式如：`callback({name:'winter'})`,可动态生成

- - -
## 另外一个服务器端进行跨域的方法
服务器端设置 http header

- - -
