- - -
## 执行上下文

      console.log(a); //undefined
      var a = 100;    //函数表达式无法提升
      fn('winter'); //  'winter' 24
      function fn(name) {
          console.log(this);  //window
          console.log(arguments);   //   ['winter',callee:function(){...},...]
          age = 24;
          console.log(name,age);
          var age;
      }
    
- 范围：一段`<script>`或者一个函数
- 全局：变量定义，函数声明（`<script>`）
- 函数：变量定义，函数声明，this，arguments（函数）

> 虽然存在变量提升，函数提升。但是还是推荐先定义，后使用。增加代码可读性。

- - -
## this
`this`要在执行的时候才能确认值，在定义的时候无法确认。

    var a = {
      name: 'winter';
      fn: function() {
        console.log(this.name);
      }
    }
    a.fn();  //this === a
    a.fn.call({age:24});  //this === {age:24}
    var fn1 = a.fn;
    fn1();   //this === window

- - -
## this 的用法
- 作为构造函数使用

      function Foo(name) {
            this.name = name;
      }
      
实际上是这样的：

      function Foo(name) {
            this = {};
            this.name = name;
            return this;
      }


- 作为对象属性执行(`a.fn()`)
- 作为普通函数执行(`var fn1 = a.fn`)
- call  apply  bind

      function fn1(name,age) {
            alert(name,age);
            console.log(this);
      }
      fn1.call({o:'summer'},'winter',24);  //this === {o:'summer'}
      fn1.apply({o:'summer'},['winter',24]);   //this === {o:'summer'}

      var fn2 = function (name,age) {
            alert(name,age);
            console.log(this);
      }.bind({o:'summer'})
      fn2('winter',24)

> 只有函数表达式可以bind(),如果是函数声明是不可以的。
- - -
## JS是解释性语言
如果一个函数内定义有错误，不会报错。只有在函数执行的时候才会报错，这就是因为JS是解释型  语言，而不是编译型语言

- - -













