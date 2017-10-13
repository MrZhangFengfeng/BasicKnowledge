## this
全局的`this`

    this.document === document;
    this === window;
    this.a = 66;
    console.log(window.a); //66

- - -
## 一般函数中的this(浏览器，非node)

    function f1(){
      return this;
    }
    f1() === window; //true


    function f1(){
      'use strict';
      return this;
    }
    f1() === window; //false
    f1() === undefined; //true

- - -
## 函数属性 和 arguments

     function foo(x,y,z){
        arguments.length; //2
        arguments[0]; //1

        arguments[0] = 10;
        x; //change to 10

        arguments[2] = 99;
        z; //still undefined.实际传参了，可以绑定修改。实际没有传参的话，无法修改。

        arguments.callee === foo; //true
     }

     foo(1,2);
     foo.length; //形参的长度，3。 和实际传入多少没关系
     arguments.length; //实参个数，2
     foo.name; //foo

- - -
## apply 和 call
    function(x,y){
      console.log(x,y,this);
    }

    foo.call(100,1,2); //1,2,Number(100)
    foo.call(true,1,2); //1,2,Boolean(true)
    foo.call(null); //undefined,undefined,**window**
    foo.call(undefined); //undefined,undefined,**window**

- - -
## bind

    this.a = 9;
    let module = {
      a: 66;
      getA : function(){return this.a}
    }

    module.getA(); //66
    let getA = module.getA;
    getA(); //9

    let bindModuleA = getA.bind(module);
    bindModuleA(); //66

- - -
## bind 和 currying

    function add(a,b,c) {
        return a + b + c;
    }
    let fn1 = add.bind(undefined,100);
    fn1(1,2); //100+1+2=103;
    
    let fn2 = fn1.bind(undefined,200);
    fn1(6); //100+200+6=306;

- - -
## bind 和 new(有待研究)
    
    function foo() {
        this.b = 100;
        return this.a;
    }
    
    let func = foo.bind({a:1});
    func(); //1
    new func(); //{b: 100}
    
- - -
