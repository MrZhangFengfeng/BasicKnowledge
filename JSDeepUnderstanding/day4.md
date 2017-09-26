## try-catch
对于嵌套式的try-catch，如果在内部已经把异常处理了，那么外面一层的catch就不会执行了。

- - -
## for in 的坑

    let p;
    let obj = {x:1,y:2}
    for(p in obj){}

- 顺序不确定
- 如果目标对象属性是`enumerable`为false的话，不会出现
- 受对象的原型链影响，如果对象原型链上有`enumerable`为true的，则也会被循环到

- - -
## 为何不要用with

    width(document.forms[0]){
      console.log{name.value}
    }

- 让JS引擎优化更难(with会改变this指向，每次都要考虑with后面的this指向问题)
- 可读性差
- 可被定义变量代替
- 严格模式下以及被禁用      
    
上面代码等价于

    let form = document.forms[0];
    console.log{
      form.name.value
    }

- - -
## 严格模式
严格模式是一种特殊的**执行模式**

它修复了部分语言上的不足，提供更强的错误检查并增强安全性。

函数内部的

    function fn(){
        'use strict';//如果不兼容严格模式，这句话就会被直接忽略
    }

全局的

    'use strict';//如果不兼容严格模式，这句话就会被直接忽略
    function fn(){ ...}

严格模式下禁止：

- 未声明的变量被赋值，将会报错。而不是像非严格模式一样生成一个全局变量。
- arguments变为参数的静态副本。像下面的这个arguments，不管传没传a，都不会修改a，打出来的都会是1.
- 但是这样对arguments是有影响的：

        !function(a){
            'use strict';
            arguments[0].x = 100;
            console.log(a.x) //100
        }
- - -
## arguments

    !function(a){
        arguments[0] = 100;
        console.log(a)
    }

如果传入了a，那么会被修改。但是如果没有传a，那么即使arguments[0] = 100;打出来的仍然是`undefined`

- - -
## 重复定义
    let obj = {x:1,x:2}
    console.log(obj.x)//2
