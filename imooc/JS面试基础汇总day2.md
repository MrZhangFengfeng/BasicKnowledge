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
