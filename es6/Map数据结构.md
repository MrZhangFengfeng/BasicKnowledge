# Map 结构的目的和基本用法

JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是只能用字符串当作键。这给它的使用带来了很大的限制。

    var data = {};
    var element = document.getElementById("myDiv");

    data[element] = metadata;
    data["[Object HTMLDivElement]"] // metadata
    
上面代码原意是将一个 DOM 节点作为对象 data 的键，但是由于对象只接受字符串作为键名，所以 element 被自动转为字符串[Object HTMLDivElement]。

#### 为了解决这个问题，ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）
都可以当作键。也就是说，Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。

    var m = new Map();
    var o = {p: "Hello World"};

    m.set(o, "content")
    m.get(o) // "content"

    m.has(o) // true
    m.delete(o) // true
    m.has(o) // false

- - -

作为构造函数，Map 也可以接受一个数组作为参数。该数组的成员是一个个表示键值对的数组。

    var map = new Map([ ["name", "张三"], ["title", "Author"]]);

    map.size // 2
    map.has("name") // true
    map.get("name") // "张三"
    map.has("title") // true
    map.get("title") // "Author"
    
上面代码在新建 Map 实例时，就指定了两个键 name 和 title。

- 如果对同一个键多次赋值，后面的值将覆盖前面的值。
- 如果读取一个未知的键，则返回 undefined。

- - -
### 只有对同一个对象的引用，Map 结构才将其视为同一个键。

    var map = new Map();
    map.set(['a'], 555);
    map.get(['a']) // undefined
    
上面代码的 set 和 get 方法，表面是针对同一个键，但实际上这是两个值，内存地址是不一样的，因此 get 方法无法读取该键，返回 undefined。

Map 的键实际上是跟内存地址绑定的，只要内存地址不一样，就视为两个键。

- - -
如果 Map 的键是一个简单类型的值（数字、字符串、布尔值），则只要两个值严格相等，Map 将其视为一个键，包括 0 和 -0。
另外，虽然 NaN 不严格相等于自身，但 Map 将其视为同一个键。

    let map = new Map();

    map.set(NaN, 123);
    map.get(NaN) // 123

    map.set(-0, 123);
    map.get(+0) // 123

- - -
