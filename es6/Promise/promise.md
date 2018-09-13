# `Promise` 对象

>所谓`Promise`，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，`Promise` 是一个对象，从它可以获取异步操作的消息。

`Promise` 对象有以下两个特点：

1. 对象不受外界影响。`Promise` 对象代表一个异步操作，有三种状态：`pending`(进行中)、`fulfilled`(已成功)、`rejected`(已失效)。只有异步操作的结果可以决定当前是哪一个状态，任何其他操作都无法改变这个状态。
2. 一旦状态改变，就不会再变化，任何时候都可以得到这个结果。`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。

`Promise`也有一些缺点。
- 首先，无法取消`Promise`，一旦新建它就会立即执行，无法中途取消。
- 其次，如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部。
- 第三，当处于`pending`状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。


>注意，为了行文方便，本章后面的resolved统一只指fulfilled状态，不包含rejected状态。

## 基本用法

创造一个Promise实例

    const promise = new Promise((resolve,rejected)=>{
        //resolve,rejected是两个函数，由JS引擎提供，不用自己部署

        if(/*操作成功*/) {
            resolve(value)
        }else{
            rejected(error)
        }
    })

`Promise`实例生成以后，可以用`then`方法分别指定`resolved`状态和`rejected`状态的回调函数。  第二个`rejected`函数是可选的。

    promise.then((value)=>{
        <!-- 成功的回调函数 -->
    },(error)=>{
        <!-- 失败的回调函数 -->
    })


`Promise` 新建后就会立即执行。

    let promise = new Promise(function(resolve, reject) {
        console.log('Promise  Start 111');
        resolve();
    });

    promise.then(function() {
        console.log('resolved. 222');
    });

    console.log('Hi! 333');

    // Promise  Start 111
    // Hi! 333
    // resolved  222


`resolve`函数的参数除了正常的值以外，还可能是另一个 `Promise` 实例，比如像下面这样。

    const p1 = new Promise(function (resolve, reject) {
        // ...
    });

    const p2 = new Promise(function (resolve, reject) {
        // ...
        resolve(p1);
    })

上面代码中，`p1`和`p2`都是 `Promise` 的实例，但是`p2`的`resolve`方法将`p1`作为参数，即一个异步操作的结果是返回另一个异步操作。

注意，这时`p1`的状态就会传递给`p2`，也就是说，`p1`的状态决定了`p2`的状态。如果`p1`的状态是`pending`，那么`p2`的回调函数就会等待`p1`的状态改变；如果`p1`的状态已经是`resolved`或者`rejected`，那么`p2`的回调函数将会立刻执行。

---
    const p1 = new Promise(function (resolve, reject) {
        setTimeout(() => reject(new Error('fail')), 3000)
    })

    const p2 = new Promise(function (resolve, reject) {
        setTimeout(() => resolve(p1), 1000)
    })

    p2.then(result => console.log(result)).catch(error => console.log(error))
    // Error: fail

上面代码中，`p1`是一个 `Promise`，3 秒之后变为`rejected`。p2的状态在 1 秒之后改变，`resolve`方法返回的是`p1`。由于`p2`返回的是另一个 `Promise`，导致`p2`自己的状态无效了，由`p1`的状态决定`p2`的状态。所以，后面的`then`语句都变成针对后者（`p1`）。又过了 2 秒，`p1`变为`rejected`，导致触发`catch`方法指定的回调函数。

---
调用resolve或reject并不会终结 Promise 的参数函数的执行。

    new Promise((resolve, reject) => {
        resolve(1);
        console.log(2);
    }).then(r => {
        console.log(r);
    });
    // 2
    // 1


上面代码中，调用`resolve(1)`以后，后面的`console.log(2)`还是会执行，并且会首先打印出来。这是因为立即 `resolved` 的 `Promise` 是在本轮事件循环的末尾执行，总是晚于本轮循环的同步任务。

---
一般来说，调用`resolve`或`reject`以后，`Promise` 的使命就完成了，后继操作应该放到`then`方法里面，而不应该直接写在`resolve`或`reject`的后面。所以，最好在它们前面加上`return`语句，这样就不会有意外。

    new Promise((resolve, reject) => {
        return resolve(1);
        // 后面的语句不会执行
        console.log(2);
    })
