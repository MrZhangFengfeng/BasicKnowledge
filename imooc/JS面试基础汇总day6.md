## 先看题目
- 说一下对变量提升的理解

      变量定义
      函数声明(说出和函数表达式的区别)
    
- 说明this几种不同的使用场景

      作为构造函数使用
      作为对象的属性执行
      作为普通函数执行
      call apply bind
      
- 创建10个a标签，点击的时候弹出对应的序号
> 还是看点击的时候i是自由变量，向上寻找，当时定义的时候上级i就是当时循环的i。

      var i;
      for(i = 0;i<10;i++) {
        (function(i) {
          a = document.createElement('a');
          a.innerHTML = i + '<br>';
          a.addEventListener('click',function(e) {
            e.preventDefault();
            alert(i)
          })
          document.body.appendChild(a);
        })(i)       
      }
      
      
- 如何理解作用域

      自由变量
      作用域链，即自由变量的查找
      闭包的两个场景

- 实际开发中闭包的应用

主要用于封装变量,收敛权限

      function isFirstLoad() {
         var _list = [];
         return function(id) {
          if(_list.indexOf(id) >= 0) {
            return false;
          }else {
            _list.push(id);
            return true;
          }
         }
      }

> 使用(先定义一个大的函数将`_list`包起来，然后返回函数，将作用域保存在定义的地方)

> 这里` _list` 是自由变量

      
      var firstLoad = isFirstLoad();
      firstLoad(10);  //true
      firstLoad(10);  //false
      firstLoad(20);  //true
 
 > 好处： 不会被人在外部随意修改` _list` 的值
