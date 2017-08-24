## es6中let的作用域和变量申明

**很多语言中都有块级作用域，但JS没有**，它使用var声明变量，以function来划分作用域，大括号“{}” 却限定不了var的作用域。
用var声明的变量具有变量提升（declaration hoisting）的效果。

#### ES6里增加了一个`let`，可以在`{}`， `if`， `for`里声明。用法同`var`，但作用域限定在块级，`let`声明的变量不存在变量提升。

- - - 
### 示例1: 块级作用域 if

    function getVal(boo) {
        if (boo) {
            var val = 'red'
            // ...
            return val
        } else {
            // 这里可以访问 val
            return null
        }
        // 这里也可以访问 val
    }
    
变量val在if块里声明的，但在else块和if外都可以访问到val。

 

把var换成let，就变成这样了

    function getVal(boo) {
        if (boo) {
            let val = 'red'
            // ...
            return val
        } else {
            // 这里访问不到 val
            return null
        }
        // 这里也访问不到 val
    }
    
- - - 
### 示例2: 块级作用域 for

    function func(arr) {
        for (var i = 0; i < arr.length; i++) {
            // i ...
        }
        // 这里也可以访问到i
    }
    
变量i在for块里声明的，但在for外也能访问到。

把var换成let，for外就访问不了i

    function func(arr) {
        for (let i = 0; i < arr.length; i++) {
            // i ...
        }
        // 这里访问不到i
    }
    
- - - 
### 示例3: 变量提升（先使用后声明）

    function func() {
        // val先使用后声明，不报错
        alert(val) // undefined
        var val;
    }
    
变量val先使用后声明，输出undefined，也不报错。

把var换成let，就报错了

    function func() {
        // val先使用后声明，报语法错
        alert(val)
        let val;
    }

- - - 
### 示例4: 变量提升（先判断后声明）

    function func() {
        if (typeof val == 'undefined') {
            // ...
        }
        var val = ''
    }
    
使用typeof判断时也可以再var语句的前面

但把var换成let，if处报语法错

    function func() {
        if (typeof val == 'undefined') {
            // ...
        }
        let val = '';
    }
    
ES6规定，如果代码块中存在let，这个区块从一开始就形成了封闭作用域。凡是在声明之前就使用，就会报错。即在代码块内，在let声明之前使用变量都是不可用的。
语法上有个术语叫“暂时性死区”（temporal dead zone），简称TDZ。当然TDZ并没有出现在ES规范里，它只是用来形象的描述。

- - - 
## let的使用注意事项
#### 1. 不能重复声明
// var和let重复声明

    var name = 'Jack';
    let  name = 'John';
 
// 两个let重复声明

    let age = 24;
    let age = 30;
    
执行时报语法错
- - -

#### 2. 有了let后，匿名函数自执行就可以去掉了
// 匿名函数写法

    (function () {
      var jQuery = function() {};
      // ...
      window.$ = jQuery
    })();
 
// 块级作用域写法

    {
      let jQuery = function() {};
      // ...
      window.$ = jQuery;
    }

- - - 
### This is `Let`
## OVER
