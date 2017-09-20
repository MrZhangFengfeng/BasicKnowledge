## 类型检测
检测方法

- typeof 

用于判断基本类型和函数对象比较好用，但是无法判断一个对象是否是数组等。。。就不好用了
    
- instanceof 

常用于判断对象。instanceof是基于原型链的操作符。

    obj instanceof Object
    
它希望左操作数是一个对象(如果不是对象，是基本类型的比如123，"winter"等 就会直接返回false)；

希望右操作数是一个函数对象，或者函数构造器(如果不是就会跑出一个`Type Error` 异常).

`instanceof` 原理就是会判断做操作数的原形链上是否有这个构造函数的prototype属性。

- - -

- Object.prototype.toString

- constructor
- duck type
