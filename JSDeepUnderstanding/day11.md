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
## 
