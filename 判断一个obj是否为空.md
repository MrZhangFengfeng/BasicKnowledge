## 判断一个obj是否为空

### 首先说下null与undefined区别：

对已声明但未初始化的和未声明的变量执行typeof，都返回"undefined"。

`null`表示一个空对象指针，`typeof`操作会返回`object`。

一般不显式的把变量的值设置为undefined，但null相反，对于将要保存对象的变量，应明确的让该变量保存null值。


    var obj;
    alert(obj);  //"undefined"
    obj = null;
    alert(typeof obj);  //"object"
    alert(obj == null);  //true
    obj = {};
    alert(obj == null);  //false
    
- - -
###　检测对象是否是空对象(不包含任何可读属性)的方法。
方法既检测对象本身的属性，也检测从原型继承的属性(因此没有使hasOwnProperty)。

    function isEmpty(obj)
    {
        for (var name in obj) 
        {
            return false;
        }
        return true;
    };
    
- - - 
### 那么问题来了，这里所说的空对象，到底是 `{}` 还是 `null`????
在codePen里面打出来试试：

    var a = {};
    a.name = 'realwall';
    console.log(isEmpty(a));  //false
    console.log(isEmpty({}));  //true
    console.log(isEmpty(null));  //true

*参数为null时无语法错误，即虽然不能对null空指针对象添加属性，但可以使用`for in` 语句。*

- - -
### 下面这个方法检测是否为空对象。区别是只检测对象本身的属性，不检测从原型继承的属性

    function isOwnEmpty(obj)
    {
        for(var name in obj)
        {
            if(obj.hasOwnProperty(name))
            {
                return false;
            }
        }
        return true;
    };
    
- - -
### `{}`与`null`的区别：

    var a = {};
    var b = null;

    a.name = 'winter';
    b.name = 'summer'; //**这里会报错，b为空指针对象，不能像普通对象一样直接添加属性**。
    b = a;
    b.name = 'summer'; //此时 a 和 b 指向同一个对象。`a.name`, `b.name` 均为`summer`
    
- - - 
## Over
