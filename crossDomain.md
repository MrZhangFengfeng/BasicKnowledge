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

- - -
## 跨域的解决方法

- Jsonp

在js中，我们直接用XMLHttpRequest请求不同域上的数据时，是不可以的。但是，在页面上引入不同域上的js脚本文件却是可以的，
jsonp正是利用这个特性来实现的。

比如，有个a.html页面，它里面的代码需要利用ajax获取一个不同域上的json数据，假设这个json数据地址是
`http://example.com/data.php`, 那么a.html 中的代码就可以这样：

    <script>
        function dosth(jsondata) {
            //handler jsondata
        }
    </script>
    <script src="http://example.com/data.php?callback=dosomething"></script>

我们看到获取数据的地址后面还有一个callback参数，按惯例是用这个参数名，但是你用其他的也一样。
当然如果获取数据的jsonp地址页面不是你自己能控制的，就得按照提供数据的那一方的规定格式来操作了。

因为是当做一个js文件来引入的，所以`http://example.com/data.php` 返回的必须是一个能执行的js文件，所以这个页面的php代码可能是这样的:

    <?php>
        $callback = $_GET['callback']; //得到回调函数名
        $data = array('winter','summer'); //要返回的数据
        echo $callback.'('.json.encode($data).')'; //输出
    <?>

最终那个页面输出的结果是:

dosomething(['winter','summer'])

所以通过`http://example.com/data.php?callback=dosomething`得到的js文件，就是我们之前定义的`dosomething`函数,并且它的参数就是我们需要的json数据，这样我们就跨域获得了我们需要的数据。

### 原理小结
通过script标签引入一个js文件，这个js文件载入成功后会执行我们在url参数中指定的函数，并且会把我们需要的json数据作为参数传入。

所以jsonp是需要服务器端的页面进行相应的配合的。

当然也可以用js动态生成script标签来进行跨域操作，而不用特意的手动的书写那些script标签。
如果你的页面使用jquery，那么通过它封装的方法就能很方便的来进行jsonp操作了。

    <script>
        $.getJSON('http://example.com/data.php?callback=?' , function(jsondata){
            //处理获得的数据
        })
    </script>

原理是一样的，只不过我们不需要手动的插入script标签以及定义回掉函数。jquery会自动生成一个全局函数来替换callback=?中的问号，之后获取到数据后又会自动销毁，实际上就是起一个临时代理函数的作用。

`$.getJSON`方法会自动判断是否跨域，不跨域的话，就调用普通的ajax方法；跨域的话，则会以异步加载js文件的形式来调用jsonp的回调函数。















