## 何时使用== 何时使用===
推荐自由在要判断的对象 是null或者undefined的时候写。(JQ源码也是这么来的)

    if(obj == null){...}
    
其余全部使用`===`

- - -
## JS中有哪些内置函数？---数据封装类对象
- Obeject
- Array
- Boolean
- Number
- String
- Function
- Date
- RegExp
- Error
- Math(这个是对象，不是函数)

- - -
## 如何理解JSON
JSON不仅是一种键值对的数据格式，而且还是一个JS对象。

作为一个JS对象，常用方法有两个：

    JSON.stringify({a:20,b:30});
    JSON.parse('{"a":20,"b":30}');

- - -












