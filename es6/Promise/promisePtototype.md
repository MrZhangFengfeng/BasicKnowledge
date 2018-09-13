# `.then()`
Promise 实例具有then方法，也就是说，then方法是定义在原型对象Promise.prototype上的。它的作用是为 Promise 实例添加状态改变时的回调函数。前面说过，then方法的第一个参数是resolved状态的回调函数，第二个参数（可选）是rejected状态的回调函数。

then方法返回的是一个新的Promise实例（注意，不是原来那个Promise实例）。因此可以采用链式写法，即then方法后面再调用另一个then方法。

采用链式的then，可以指定一组按照次序调用的回调函数。这时，前一个回调函数，有可能返回的还是一个Promise对象（即有异步操作），这时后一个回调函数，就会等待该Promise对象的状态发生变化，才会被调用。

    getJSON("/post/1.json").then(
        post => getJSON(post.commentURL)
    ).then(
        comments => console.log("resolved: ", comments),
        err => console.log("rejected: ", err)
    );


# `.catch()`
`Promise.prototype.catch`方法是`.then(null, rejection)`的别名，用于指定发生错误时的回调函数。

    getJSON('/posts.json').then(function(posts) {
        // ...
    }).catch(function(error) {
        // 处理 getJSON 和 前一个回调函数运行时发生的错误
        console.log('发生错误！', error);
    });

上面代码中，`getJSON`方法返回一个 `Promise` 对象，如果该对象状态变为`resolved`，则会调用 `then` 方法指定的回调函数；如果异步操作抛出错误，状态就会变为rejected，就会调用 `catch` 方法指定的回调函数，处理这个错误。另外，`then` 方法指定的回调函数，如果运行中抛出错误，也会被 `catch` 方法捕获。

>如果 `Promise` 状态已经变成 `resolved`，再抛出错误是无效的。即，只要成功了，不会再抛出错误。

Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个`catch`语句捕获。

    getJSON('/post/1.json').then(function(post) {
        return getJSON(post.commentURL);
    }).then(function(comments) {
        // some code
    }).catch(function(error) {
        // 处理前面三个Promise产生的错误
    });

跟传统的 `try/catch` 代码块不同的是，如果没有使用 `catch` 方法指定错误处理的回调函数，`Promise` 对象抛出的错误不会传递到外层代码，即不会有任何反应。这就是说，Promise 内部的错误不会影响到 Promise 外部的代码，通俗的说法就是 `Promise 会吃掉错误`。

---
`catch` 方法返回的还是一个 `Promise` 对象，因此后面还可以接着调用 `then` 方法。

    const someAsyncThing = function() {
        return new Promise(function(resolve, reject) {
            // 下面一行会报错，因为x没有声明
            resolve(x + 2);
        });
    };

    someAsyncThing()
        .catch(function(error) {
        console.log('oh no', error);
    })
    .then(function() {
        console.log('carry on');
    });
    // oh no [ReferenceError: x is not defined]
    // carry on

---
catch方法之中，还能再抛出错误,并继续用catch继续捕获

    someAsyncThing().then(function() {
        return someOtherAsyncThing();
    }).catch(function(error) {
        console.log('oh no', error);
        // 下面一行会报错，因为y没有声明
        y + 2;
    }).catch(function(error) {
        console.log('carry on', error);
    });
    // oh no [ReferenceError: x is not defined]
    // carry on [ReferenceError: y is not defined]


# `.finally()`
`finally` 方法用于指定不管 `Promise` 对象最后状态如何，都会执行的操作。

    promise.then(result => {···})
           .catch(error => {···})
           .finally(() => {···});

`finally` 方法的回调函数不接受任何参数，这意味着没有办法知道，前面的 Promise 状态到底是 `fulfilled` 还是 `rejected` 。这表明， `finally` 方法里面的操作，应该是与状态无关的，不依赖于 `Promise` 的执行结果

# `.all()`
`Promise.all`方法用于将多个 `Promise` 实例，包装成一个新的 `Promise` 实例。

    const p = Promise.all([p1, p2, p3]);

上面代码中，`Promise.all`方法接受一个数组作为参数，`p1、p2、p3`都是 `Promise` 实例，如果不是，就会先调用下面讲到的 `Promise.resolve` 方法，将参数转为 `Promise` 实例，再进一步处理。（Promise.all方法的参数可以不是数组，但必须具有 `Iterator` 接口，且返回的每个成员都是 `Promise` 实例。）

p的状态由p1、p2、p3决定，分成两种情况。
- 只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
- 只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。


    const databasePromise = connectDatabase();

    const booksPromise = databasePromise
        .then(findAllBooks);

    const userPromise = databasePromise
        .then(getCurrentUser);

    Promise.all([
        booksPromise,
        userPromise
    ])
    .then(([books, user]) => pickTopRecommentations(books, user));

上面代码中，`booksPromise`和`userPromise`是两个异步操作，只有等到它们的结果都返回了，才会触发`pickTopRecommentations`这个回调函数。

>注意，如果作为参数的 `Promise` 实例，自己定义了`catch`方法，那么它一旦被`rejected`，并不会触发`Promise.all()`的`catch`方法。

---
    const p1 = new Promise((resolve, reject) => {
        resolve('hello');
    })
    .then(result => result)
    .catch(e => e);

    const p2 = new Promise((resolve, reject) => {
        throw new Error('报错了');
    })
    .then(result => result)
    .catch(e => e);

    Promise.all([p1, p2])
        .then(result => console.log(result))
        .catch(e => console.log(e));
    // ["hello", Error: 报错了]

上面代码中，p1会`resolved`，p2首先会`rejected`，但是p2有自己的`catch`方法，该方法返回的是一个新的 `Promise` 实例，p2指向的实际上是这个实例。该实例执行完`catch`方法后，也会变成`resolved`，导致`Promise.all()`方法参数里面的两个实例都会`resolved`，因此会调用`then`方法指定的回调函数，而不会调用`catch`方法指定的回调函数。

如果p2没有自己的`catch`方法，就会调用`Promise.all()`的`catch`方法。


# `.race()`
`Promise.race`方法同样是将多个 `Promise` 实例，包装成一个新的 `Promise` 实例。

    const p = Promise.race([p1, p2, p3]);

上面代码中，只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 `Promise` 实例的返回值，就传递给p的回调函数。如果指定时间内没有获得结果，就将 `Promise` 的状态变为`reject`，否则变为`resolve`。

# `.resolve()`
`Promise.resolve` 方法将对象转为 `Promise` 对象。Promise.resolve等价于下面的写法。

    Promise.resolve('foo')
    // 等价于
    new Promise(resolve => resolve('foo'))


Promise.resolve方法的参数分成四种情况。

（1）参数是一个 Promise 实例

如果参数是 Promise 实例，那么Promise.resolve将不做任何修改、原封不动地返回这个实例。

（2）参数是一个thenable对象

thenable对象指的是具有then方法的对象，比如下面这个对象。

    let thenable = {
        then: function(resolve, reject) {
            resolve(42);
        }
    };
Promise.resolve方法会将这个对象转为 Promise 对象，然后就立即执行thenable对象的then方法。

    let thenable = {
        then: function(resolve, reject) {
            resolve(42);
        }
    };

    let p1 = Promise.resolve(thenable);
        p1.then(function(value) {
        console.log(value);  // 42
    });

上面代码中，thenable对象的then方法执行后，对象p1的状态就变为resolved，从而立即执行最后那个then方法指定的回调函数，输出 42。

（3）参数不是具有then方法的对象，或根本就不是对象

如果参数是一个原始值，或者是一个不具有then方法的对象，则Promise.resolve方法返回一个新的 Promise 对象，状态为resolved。

    const p = Promise.resolve('Hello');

    p.then((value)=> {
        console.log(value)
    });
    // Hello

（4）不带有任何参数

Promise.resolve方法允许调用时不带参数，直接返回一个resolved状态的 Promise 对象。

所以，如果希望得到一个 Promise 对象，比较方便的方法就是直接调用Promise.resolve方法。

    const p = Promise.resolve();

    p.then(function () {
        // ...
    });

上面代码的变量p就是一个 Promise 对象。

需要注意的是，立即resolve的 Promise 对象，是在本轮“事件循环”（event loop）的结束时，而不是在下一轮“事件循环”的开始时。

    setTimeout(function () {
        console.log('three');
    }, 0);
    Promise.resolve().then(function () {
        console.log('two');
    });
    console.log('one');

    // one
    // two
    // three

上面代码中，`setTimeout(fn, 0)`在下一轮“事件循环”开始时执行，`Promise.resolve()`在本轮“事件循环”结束时执行，`console.log('one')`则是立即执行，因此最先输出。


# `.reject()`
`Promise.reject(reason)`方法也会返回一个新的 `Promise` 实例，该实例的状态为`rejected`。

    const p = Promise.reject('出错了');
    // 等同于
    const p = new Promise((resolve, reject) => reject('出错了'))

    p.then(null, function (s) {
        console.log(s)
    });
    // 出错了

上面代码生成一个 Promise 对象的实例p，状态为rejected，回调函数会立即执行。

>注意，Promise.reject()方法的参数，会原封不动地作为reject的理由，变成后续方法的参数。这一点与Promise.resolve方法不一致。

    const thenable = {
        then(resolve, reject) {
            reject('出错了');
        }
    };

    Promise.reject(thenable)
        .catch(e => {
        console.log(e === thenable)
    })
    // true

上面代码中，Promise.reject方法的参数是一个thenable对象，执行以后，后面catch方法的参数不是reject抛出的“出错了”这个字符串，而是thenable对象。


# `.try()`
