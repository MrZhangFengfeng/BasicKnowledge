> for...of语句在可迭代对象(包括 Array, Map, Set, String, TypedArray，arguments 对象等等)上创建一个迭代循环，
对每个不同属性的属性值,调用一个自定义的有执行语句的迭代挂钩.

## 语法

    for (variable of iterable) {
        //statements
    }

- - -
## 例子
#### 迭代 Array:

    let iterable = [10, 20, 30];

    for (let value of iterable) {
        value += 1;
        console.log(value);
    }
    // 11
    // 21
    // 31

- - -
如果你不修改语句块中的变量 , 也可以使用 const 代替 let .

    let iterable = [10, 20, 30];

    for (const value of iterable) {
      console.log(value);
    }
    // 10
    // 20
    // 30

- - -
## 迭代 String:
    let iterable = "boo";

    for (let value of iterable) {
      console.log(value);
    }
    // "b"
    // "o"
    // "o"

- - -
## 迭代 TypedArray:
    let iterable = new Uint8Array([0x00, 0xff]);

    for (let value of iterable) {
      console.log(value);
    }
    // 0
    // 255

- - -
## 迭代Map:
    let iterable = new Map([["a", 1], ["b", 2], ["c", 3]]);

    for (let entry of iterable) {
      console.log(entry);
    }
    // ["a", 1]
    // ["b", 2]
    // ["c", 3]

    for (let [key, value] of iterable) {
      console.log(value);
    }
    // 1
    // 2
    // 3

- - -
## 迭代 Set:
    let iterable = new Set([1, 1, 2, 2, 3, 3]);

    for (let value of iterable) {
      console.log(value);
    }
    // 1
    // 2
    // 3  不会打出重复的元素

- - -
## 迭代参数对象
    (function() {
      for (let argument of arguments) {
        console.log(argument);
      }
    })(1, 2, 3);

    // 1
    // 2
    // 3

- - -
## 迭代 DOM 集合
迭代 DOM 元素集合,比如一个NodeList对象：
> 注意：这只能在实现了NodeList.prototype [Symbol.iterator]的平台上运行

    let articleParagraphs = document.querySelectorAll("article > p");

    for (let paragraph of articleParagraphs) {
      paragraph.classList.add("read");
    }

- - -
## 关闭迭代器
对于`for...of` 的循环，突然的迭代终止可以由`break`，`continue`，`throw`或`return`引起。在这些情况下，迭代器关闭。

    function* foo(){ 
      yield 1; 
      yield 2; 
      yield 3; 
    }; 

    for (let o of foo()) { 
      console.log(o); 
      break; // closes iterator, triggers return
    }
    // 1

**function后面要加`*`,否则报错**

- - -
# for...of与for...in的区别

- for...in循环会遍历一个object所有的可枚举属性。

- for...of语法是为各种collection对象专门定制的，并不适用于所有的object.它会以这种方式迭代出任何拥有[Symbol.iterator] 属性
的collection对象的每个元素。

下面的例子演示了for...of 循环和 for...in 循环的区别。for...in 遍历（当前对象及其原型上的）每一个属性名称,
而 for...of遍历（当前对象上的）每一个属性值:

    Object.prototype.objCustom = function () {}; 
    Array.prototype.arrCustom = function () {};

    let iterable = [3, 5, 7];
    iterable.foo = "hello";

    for (let i in iterable) {
      console.log(i); 
      // logs 0, 1, 2, "foo", "arrCustom", "objCustom"
    }

    for (let i of iterable) {
      console.log(i); // logs 3, 5, 7
    }

- - -
# OVER
