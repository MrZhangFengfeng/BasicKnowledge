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

在控制台输出后得到：

        Number
        ƒ Number() { [native code] }
        Array
        ƒ Array() { [native code] }        
        Function
        ƒ Function() { [native code] }
        Math
        Math {abs: ƒ, acos: ƒ, acosh: ƒ, asin: ƒ, asinh: ƒ, …}
        JSON
        JSON {Symbol(Symbol.toStringTag): "JSON", parse: ƒ, stringify: ƒ}
        
- - -
## 如何理解JSON
JSON不仅是一种键值对的数据格式，而且还是一个JS对象。

作为一个JS对象，常用方法有两个：

    JSON.stringify({a:20,b:30});
    JSON.parse('{"a":20,"b":30}');

- - -
## 杂七杂八
引用类型的扩展

    var a = [1,2,3]
    a.age = 24;
    a; //[1,2,3,age:24]











