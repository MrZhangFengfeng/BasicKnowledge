## 原型和原型链相关题目

- 如何准确判断一个对象是数组类型
- 写一个原型链继承的例子
- 描述new一个对象的过程
- zepto(或者其他源码)中如何使用原型链

- - -
## 原型链知识点
- 构造函数(开头大写)---构造函数类似于提供一个模板

        function Foo(name,age){
            this.name = name;
            this.age = age;
            this.class = 'class-1';
            //return this;  默认有这一行
        }
        var f = new Foo('winter',20); 
        f:      Object {
                  age: 20,
                  class: "class-1",
                  name: "winter"
                }

- 构造函数--扩展

        var a = {}; 其实是var a = new Object()的语法糖。
        var b = []; 其实是var b = new Array()的语法糖。
        function C(){...}; 其实是var C = new Function()的语法糖。
        
        a的构造函数是  Object()
        b的构造函数是  Array()
        C的构造函数是  Function()
        
- 原型规则和示例


- 原型链


- instanceof
使用instanceof来判断一个 函数 是否是一个 对象 的构造函数

- - -
## 杂七杂八
- JS自定义函数中默认返回 undefined









