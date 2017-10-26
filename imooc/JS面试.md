## 先来几道面试题
- JS中typeof能得到哪几种类型？
    - 考点：JS变量类型
    
- 何时使用`===`？何时使用`==`？
    - 考点：强制类型转换
    
- `window.onload` 和`DOMContentLoaded`的区别
    - 考点：浏览器渲染过程
    
- 用JS创建10个a标签，点击的时候弹出对应的序号
    - 考点：作用域
    
- 简述如何实现一个模块加载器，实现类似`require.js`的基本功能
    - 考点：JS模块化
    
- 实现数组的随机排序
    - 考点：JS基础算法

- JS中有哪些内置函数
- JS变量按照存储方式分为哪些类型，并描述其特点
- 如何理解JSON

- - -
## 变量类型
- 值类型VS引用类型

值类型(a,b存储在不同位置,Number,String,Boolean,Null,Undefined)：

    let a = 100;
    let b = a;
    a = 200;
    console.log(b) //100
    
引用类型(对象，数组和函数)：

    var a = {age: 24};
    var b = a;
    b.age = 18;
    console.log(a.age); //18
    
- typeof运算符详解
