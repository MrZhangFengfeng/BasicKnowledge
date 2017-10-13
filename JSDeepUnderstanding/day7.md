## 对象属性

- [[proto]]
- [[class]] //指的是对象类型。数字，字符串，布尔值
- [[extensible]]

      let obj = {x: 1,y :2};
      Object.isExtensible(obj); //true
      Object.preventExtensions(obj); 
      Object.isExtensible(obj); //false

      obj.z = 1;
      obj.z; //因为是不能扩展,所以得到 undefined,add new property failed

      Object.seal(obj); //将obj的configurable改为false;
      Object.isSealed(obj); //true

      Object.freeze(obj); //将obj的configurable 和 writable同时改为false;
      Object.isFrozen(obj); //true

      seal 和 freeze都不会影响原型链。
      
- - -
## 序列化
      let obj = {x: 1,y: true};
      JSON.stringify(obj); //"{'x': 1,'y': true}";

      let obj = {val: undefined,x: NaN, y: Infinity,z: new Date()};
      JSON.stringify(obj); //"{'x':null,'y':null,'z':'2017-10-12T15:12:26.852Z'}"   val直接不显示了

      obj = JSON.parse("{'x': 1,'y': true}");
      obj.x; //1

- - -
## 序列化-自定义
      let obj = {
            x: 1,
            o: {
                  a: 1,
                  b: 2,
                  toJSON: function() {
                        return this.a + this.b;
                  }
            }
      }
      JSON.stringify(obj); // "{'x':1,'o':3}"

> 注意： toJSON 方法名是固定的 不可变的。
