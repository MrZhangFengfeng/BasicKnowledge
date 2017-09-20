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

> 不同的**window**或者**iframe**之间的对象类型检测**不能**使用`instanceof`。会返回false。

- - -

- Object.prototype.toString

        Object.prototype.toString.apply([]);//==="[object Array]"
        Object.prototype.toString.apply(function(){});//==="[object Function]"
        Object.prototype.toString.apply(null);//==="[object Null]"
        Object.prototype.toString.apply(underfined);//==="[object Underfined]"

> IE678下：Object.prototype.toString.apply(null);//==="[object Object]"

- - -

- constructor
任何对象都有一个`constructor`属性，是由原型继承来的，`constructor`会指向构造这个对象的构造器。
`constructor`能够被改写，所以要注意。
- - -
- duck type
通过对象的属性，方法等来判断。比如一个对象是否有数组该有的长度啊，push方法啊 等等

- - -
## 类型检测小结

- typeof：适合基本类型及function检测，遇到null失效。
- Object.prototype.toString.apply()：适合内置对象和基本元素类型，遇到null和underfined失效。(IE678)
- instanceof：适合自定义对象，也可以用来检测原始对象，不同窗口下失效。

- - -
- - -

