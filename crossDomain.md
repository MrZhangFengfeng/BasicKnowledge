## 什么是跨域？

跨域，指的是浏览器不能执行其他网站的脚本。它是由浏览器的同源策略造成的，是浏览器对javascript施加的安全限制。

例子：

- http://www.123.com/index.html 调用 
- http://www.123.com/server.php 
- （非跨域）

- - -

- http://www.123.com/a/a.js 调用 
- http://www.123.com/b/b.js 
- （同一个域名下不同文件夹，非跨域）

- - -

- http://www.123.com:8080/a.js 调用 
- http://www.123.com/b.js 
- （端口不同，跨域）

- - -

- http://www.123.com/a.js 调用 
- http://70.32.64.95/b.js 
- （域名和其对应的IP地址，也是跨域）

- - -

- http://www.123.com/a.js 调用 
- http://winter.123.com/b.js 
- （主域名相同，子域名不同，跨域）

- - -

- http://www.123.com/a.js 调用 
- http://123.com/b.js 
- （一域名，不同二级域名，跨域）

- - -

- http://abc.123.com/index.html 调用 
- http://def.123.com/server.php 
- （子域名不同:abc/def，跨域）

- - -

> 请注意：localhost和127.0.0.1虽然都指向本机，但也属于跨域。


