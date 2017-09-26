## try-catch
对于嵌套式的try-catch，如果在内部已经把异常处理了，那么外面一层的catch就不会执行了。

- - -
## for in 的坑

    let p;
    let obj = {x:1,y:2}
    for(p in obj){}

- 顺序不确定
- 如果目标对象属性是`enumerable`为false的话，不会出现
- 受对象的原型链影响，如果对象原型链上有`enumerable`为true的，则也会被循环到

- - -
## 为何不要用with

    width(document.forms[0]){
      console.log{name.value}
    }

- 让JS引擎优化更难(with会改变this指向，每次都要考虑with后面的this指向问题)
- 可读性差
- 可被定义变量代替
- 严格模式下以及被禁用      
    
上面代码等价于

    let form = document.forms[0];
    console.log{
      form.name.value
    }
