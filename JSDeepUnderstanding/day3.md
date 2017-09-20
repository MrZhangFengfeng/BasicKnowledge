## JavaScript表达式

### delete运算符

    var obj = {x:1};
    obj.x; //1
    delete obj.x;
    obj.x; //undefined

可以通过设置来禁止delete

    var obj;
    Object.definProperty(obj,'x',{
        configurable:false,
        value:1
    })
    
    obj.x; //1
    delete obj.x; //false
    obj.x; //1
    
- - -
### in 运算符

    window.a=1;
    a in window //true
    
- - -
### new运算符

    function Fun(){};
    Foo.prototype.x=1;
    let obj = new Foo();
    obj.x; //1
    
    obj._proto_.hasOwnProperty('x'); //true
    
- - -
### void运算符

    void 0;   //undefined
    void(0);  //undefined
    
- - -
- - -
## JavaScript语句

如何不让b成为全局变量？用逗号分隔： `var a =1,b=1;`







