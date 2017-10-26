## JS中没有块级作用域
函数作用域，在定义的时候就已经确定。

- - -
## 自由变量：当前作用域没有定义的变量，叫做自由变量。

    var a = 100;
    function Fn1() {
      var b = 200;
      console.log(a);  //此时a是自由变量
      function Fn2() {
        var c = 300;
        console.log(a); //此时a是自由变量
        console.log(b); //此时b是自由变量
        console.log(c); 
      }
      Fn2();
    }
    Fn1();


- - -
## 闭包
    function F1() {
      var a = 100;
      return function() {
        console.log(a);
      }
    }

    var f1 = new F1();
    var a = 200;
    f1();  //100

解释

    f1 = function() {
      console.log(a)
    }

这里的a是自由变量，不在当前作用域，就会沿着作用域链向上一层寻找。而作用域是在定义的时候就已经确定了，所以就会找到100。
