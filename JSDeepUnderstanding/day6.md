## get/set与原型链

    function foo(){}
    Object.defineProperty(foo.prototype,'z',{get:function(){return 1;}});

    let obj = new foo();

    obj.z; //1
    obj.z = 10; //still 1,不能改变

    Object.defineProperty(obj,'z',{value: 100,configurable: true});
    obj.z; //100
    delete obj.z; //OK
    obj.z; // back to 1; (foo.prototype)
    
- - -
//默认configurable: false
    
    Object.defineProperty(obj,'z',{value: 100}); 
    obj.z= 2;//false
    delete obj.z; //false

- - -
## 属性标签

    Object.getOwnPropertyDescriptor({name:winter},'name');
    //Object{value: winter,writable: true,enumerable: true,configurable: true}
    Object.getOwnPropertyDescriptor({name:winter},'age');
    //undefined

    Object.defineProperty(person,'name',{
      configurable: false,
      enumerable: false,
      writable: false,
      value: 'winter'
    });

    Object.defineProperty(person,{
        name:{...},
        age:{...}
    }});
- - -
## .keys
example:

    var arr = ['a', 'b', 'c'];
    console.log(Object.keys(arr)); // console: ['0', '1', '2']

    // array like object
    var obj = { 0: 'a', 1: 'b', 2: 'c' };
    console.log(Object.keys(obj)); // console: ['0', '1', '2']

    // array like object with random key ordering
    var anObj = { 100: 'a', 2: 'b', 7: 'c' };
    console.log(Object.keys(anObj)); // ['2', '7', '100']

    // getFoo is property which isn't enumerable
    var myObj = Object.create({}, {
      getFoo: {
        value: function () { return this.foo; }
      } 
    });
    myObj.foo = 1;
    console.log(Object.keys(myObj)); // console: ['foo']

- - -
## 
