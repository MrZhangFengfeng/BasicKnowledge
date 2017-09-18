## JS定义变量无需指定类型

    let num = 123
    num = "I am a string";

这样也没问题。

- - -
## JS六种数据类型

- 五种基本数据类型（原始类型）
    - undefined
    - null
    - boolean
    - string
    - num
    
- 复杂数据类型（对象类型）
    - object

> function  array  date 等等都属于 object

- - -
## Tips

    "37" -7 --->  30
    string - 0 --->  string类型转为number类型

- - -
## `==`运算符
eg.

    "1.23" == 1.23
    0 == false
    null == undefined
    
    new Object() != new Object()
    [1,2] != [1,2]

原理
类型不同，尝试类型转换再比较

    boolean == ? 布尔值不管和什么比，都会先将 true转为1，false转为0 ，然后再进行比较。 1 == true  //true
    
    object == number|string  尝试对象转为基本类型   new String("hi") == hi //true
    
 - - -
 ## JS中的包装对象
 
     let str = "winter";
     console.log(str) ------------->"winter"
     console.log(str.length)    //6
     str.name="zxf"
     console.log(str.name)     //undefined

     let str2 = new String("summer")
     console.log(str) ------------->String{0:"s",1:"u",2...,length:6,[[PrimitiveValue]]:"string"}
 
 > 基本类型应该是没有属性和方法的。
 
 > 如果尝试对一个基本类型添加方法和属性，JS会自动将这个基本类型转为包装类型，但是一旦你赋值结束，这个临时的包装类型就会被销毁。
 
 > `Number Object`  和 `Boolean Object` 也一样。
 
 - - -
 
 
 
