## 原型和原型链相关题目

- 如何准确判断一个对象是数组类型

        答案： obj instanceof Array
        
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
        
- 原型链


- instanceof
使用instanceof来判断一个 函数 是否是一个 对象 的构造函数

- - -
## 原型规则
- 1、所有的引用类型都具有对象的特性，即可自由扩展属性。除了null以外。
> 值类型不可以，即使添加了，也还是undefined。

- 2、所有的**引用类型**都具有一个`_proto_`属性，属性值是一个普通的对象
> `_proto_`: 隐式原型

- 3、所有的**函数**都具有一个`prototype`属性，属性值也是一个普通的对象
> `prototype`: 显式原型

- 4、所有的引用类型的`_proto_`**属性值**都指向他的构造函数的`prototype`**属性值**。

        var obj = {};
        obj._proto_  === Object.prototype
        
- - -
## 杂七杂八
- JS自定义函数中默认返回 undefined

- - -








