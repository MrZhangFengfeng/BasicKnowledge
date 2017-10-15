## 对象的方法--toString

    let obj = {x: 1,y: 2};
    obj.toString();  //本身的方法："[object Object]"
    obj.toString = function(){ return this.x + this.y };  //重写toString方法
    "Result " + obj.toString(); //"Result 3"
    +obj.toString(); //数字  3
    
    obj.valueOf() //得到如下：
    
        Object{
          toString: function(){...},
          x: 1,
          y: 2
        }
    
    obj.valueOf =  function(){rturn this.x + this.y} //重写valueOf方法
- - -
## 对象的方法--__proto__

    let obj = {x: 1,y: 2};
    obj.__proto__; //Object {}
    Object.getPrototypeOf(obj) === Object.prototype;  // true

    function foo() {...}
    foo.prototype.__proto__; //Object {}
    foo.prototype.__proto__ === Object.prototype; // true

就是因为上述的原因，我们才能使用toString,valueOf等内置方法，因为都是继承自`Object.prototype`的。

- - -
## 注意，不是所有的对象原型链上都有`Object.prototype`
例如：

    let obj2 = Object.create(null);
    obj2.__proto__; //undefined
    obj2.toString; //undefined

- - -
## 再注意，不是所有的对象`prototype`这个属性
例如：

    function abd() {...}
    abc.prototype; //Object {}

    let abcBinded = abc.bind(null);
    abcBinded.prototype; //undefined
