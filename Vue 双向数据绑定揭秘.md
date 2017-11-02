# Vue 双向数据绑定揭秘

> AngularJS 采用“脏值检测”的方式，数据发生变更后，对于所有的数据和视图的绑定关系进行一次检测，识别是否有数据发生了改变，
> 有变化进行处理，可能进一步引发其他数据的改变，所以这个过程可能会循环几次，一直到不再有数据变化发生后，将变更的数据发送到视图，
> 更新页面展现。如果是手动对 ViewModel 的数据进行变更，为确保变更同步到视图，需要手动触发一次“脏值检测”。

- - -
## Vue 双向数据绑定实现
数据与视图的绑定与同步，最终体现在对数据的读写处理过程中，也就是 `1Object.defineProperty()` 定义的数据 set、get 函数中。
Vue 中对于的函数为 `defineReactive`，下面的代码是只保留了基本特性的简约版实现：

    function defineReactive(obj, key, value){
         var dep = new Dep();
         Object.defineProperty(obj, key, {
             enumerable: true,
             configurable: true,
             get: function reactGetter(){
                 if(Dep.target){
                     dep.depend();
                 }
                 return value;
             },
             set: function reactSetter(newVal){
                 if (value === newVal) {
                     return;
                 } else {
                      value = newVal;
                      //如果数据发生改变，则通知所有的 watcher（借助 dep.notify()）
                      dep.notify();
                 }
             }
         })
     }

在对数据进行读取时，如果当前有 Watcher（对数据的观察者，watcher 会负责将获取的新数据发送给视图），
那将该 Watcher 绑定到当前的数据上（dep.depend()，dep 关联当前数据和所有的 watcher 的依赖关系），
是一个检查并记录依赖的过程。而在对数据进行赋值时，如果数据发生改变，则通知所有的 watcher（借助 dep.notify()）。
这样，即便是我们手动改变了数据，框架也能够自动将数据同步到视图

- - -
## 附：React数据绑定

React采用这种方式，考虑虚拟DOM树的更新：

- 属性更新，组件自己处理
- 结构更新，重新“渲染”子树（虚拟DOM），找出最小改动步骤，打包DOM操作，给真实DOM树打补丁
- 单纯从数据绑定来看，React虚拟DOM没有数据绑定，因为setState()不维护上一个状态（状态丢弃），谈不上绑定

从数据更新机制来看，React类似于提供数据模型的方式（必须通过state更新）

- - -
## OVER
