# jQuery Ajax 操作函数

jQuery 库拥有完整的 Ajax 兼容套件。其中的函数和方法允许我们在不刷新浏览器的情况下从服务器加载数据。

- - -

## jQuery.ajax()	
执行异步 HTTP (Ajax) 请求。

实例
通过 AJAX 加载一段文本：
jQuery 代码：

    $(document).ready(function(){
        $("#btn").click(function(){
          htmlobj=$.ajax({url:"/jquery/test1.txt",async:false});
          $("#myDiv").html(htmlobj.responseText);
        });
    });
    
HTML 代码：

    <div id="myDiv"><h2>Let AJAX change this text</h2></div>
    <button id="btn" type="button">Change Content</button>

定义和用法
ajax() 方法通过 HTTP 请求加载远程数据。

该方法是 jQuery 底层 AJAX 实现。$.ajax() 返回其创建的 `XMLHttpRequest` 对象。大多数情况下你无需直接操作该函数，除非你需要操作不常用的选项，
以获得更多的灵活性。最简单的情况下，$.ajax() 可以不带任何参数直接使用。

语法

    jQuery.ajax([settings])
    
`settings`	:用于配置 Ajax 请求的键值对集合。
> 注意：所有的选项都可以通过 $.ajaxSetup() 函数来全局设置。

- - -
## settings

- async: 默认值true。默认设置下，所有请求均为异步请求。如果需要发送同步请求，请将此选项设置为 false。
> 同步请求将锁住浏览器，用户其它操作必须等待请求完成才可以执行。

- beforeSend(XHR): 发送请求前可修改 XMLHttpRequest 对象的函数，如添加自定义 HTTP 头。
> XMLHttpRequest 对象是唯一的参数。
> 这是一个 Ajax 事件。如果返回 false 可以取消本次 ajax 请求。

- cache: 默认值: true，dataType 为 script 和 jsonp 时默认为 false。设置为 false 将不缓存此页面。

- complete(XHR, TS): 请求完成后回调函数 (请求成功或失败之后均调用)。
> 参数： arg1: XMLHttpRequest 对象, arg2: 一个描述请求类型的字符串。

- contentType: 默认值: "application/x-www-form-urlencoded"。发送信息至服务器时内容编码类型。
> 默认值适合大多数情况。如果你明确地传递了一个 content-type 给 $.ajax() 那么它必定会发送给服务器（即使没有数据要发送）。
