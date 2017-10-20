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

- - -

- 通过修改`document.domain`来跨子域

浏览器都有一个同源策略，其限制之一就是JSONP中我们说的不能通过ajax的方法去请求不同源中的文档。 它的第二个限制是浏览器中不同域的框架之间是不能进行js的交互操作的。

有一点需要说明，不同的框架之间（父子或同辈），是能够获取到彼此的window对象的，但是你却不能使用获取到的window对象的属性和方法，总之，你可以当做是只能获取到一个几乎无用的window对象。

> 注：html5中的postMessage方法是一个例外

比如，有一个页面，它的地址是`http://www.example.com/a.html`， 在这个页面里面有一个iframe，它的src是`http://example.com/b.html`, 很显然，这个页面与它里面的iframe框架是不同域的，所以我们是无法通过在页面中书写js代码来获取iframe中的东西的：

> contentWindow属性是指指定的frame或者iframe所在的window对象

    <script>
        function onload(){
            var iframe = $('#iframe');
            var win = iframe.contentWindow; //可以获取到iframe里的window对象，但是该window对象的属性和方法却是不可用的
            var doc = win.document; //undefined
            var name = win.name; //undefined
            ...
        }
    </script>
    <iframe id="iframe" src= "http://example.com/b.html" onload="onload()"></iframe>

这个时候，`document.domain`就可以派上用场了，我们只要把`http://www.example.com/a.html` 和 `http://example.com/b.html`这两个页面的`document.domain`都设成相同的域名就可以了!

> 注意的是，document.domain的设置是有限制的，我们只能把document.domain设置成自身或更高一级的父域，且主域必须相同。
> 例如：`a.b.example.com` 中某个文档的document.domain 可以设成 `a.b.example.com`、`b.example.com` 、`example.com` 中的任意一个，
> 但是不可以设成 `c.a.b.example.com`,因为这是当前域的子域，也不可以设成`baidu.com`,因为主域已经不相同了。

在页面 `http://www.example.com/a.html` 中设置document.domain:

    <iframe id="iframe" src= "http://example.com/b.html" onload="test()"></iframe>
    <script>
        document.domain = 'example.com'; //设置成主域
        function test(){
            alert($('#iframe').contentWindow.name,$('#iframe').contentWindow.documnet)
        }
    </script>

在页面 `http://example.com/b.html` 中也设置document.domain，而且这也是必须的，虽然这个文档的domain就是`example.com`,但是还是必须显示的设置document.domain的值：

    <script>
        document.domain = 'example.com'; //设置成主域
    </script>

这样我们就可以通过js访问到iframe中的各种属性和对象了。

##划重点！##
如果你想在`http://www.example.com/a.html` 页面中通过`ajax`直接请求`http://example.com/b.html` 页面，即使你设置了相同的document.domain也还是不行的，所以修改document.domain的方法**只适用于不同子域的框架间的交互**。

**解决方法**
如果你想通过ajax的方法去与不同子域的页面交互，除了使用jsonp的方法外，还可以用一个隐藏的iframe来做一个代理。

原理就是让这个iframe载入一个与你想要通过ajax获取数据的目标页面处在相同的域的页面，所以这个iframe中的页面是可以正常使用ajax去获取你要的数据的，
然后就是通过我们刚刚讲得修改document.domain的方法，让我们能通过js完全控制这个iframe，这样我们就可以让iframe去发送ajax请求，然后收到的数据我
们也可以获得了。

> 扶植一个傀儡iframe，让他去帮我们拿数据。

- - -
## 使用window.name来进行跨域







