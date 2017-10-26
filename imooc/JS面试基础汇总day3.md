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


- 原型规则和示例


- 原型链


- instanceof


- - -
## 杂七杂八
- JS自定义函数中默认返回 undefined









