### 字符串模板

    <i className={`icon ml20 ${this.state.isPlay ? 'pause' : 'play'}`} ></i>

- - - 
### node?
- 后端是没办法直接拿前端的接口的，这就需要一个中介，这个中介就是node,apache,ngix...
- `node`是前端可以用js来写的服务器。
- `nodeJS`的特点是事件驱动，非阻塞。


- - -
### 动画回调
- `animationstart` - CSS 动画开始后触发
- `animationiteration` - CSS 动画重复播放时触发
- `animationend` - CSS 动画完成后触发

- `transitionend` 事件在 CSS 完成过渡后触发。
> 注意： 如果过渡在完成前移除，例如 CSS `transition-property` 属性被移除，过渡事件将不被触发。

- - -
### if()
    if(0){...} //false
    if(''){...} //false

- - -
## SDK

`SDK`的英文全名是：`software development kit`，翻译成中文的意思就是“软件开发工具包”
通俗一点的理解，是指由第三方服务商提供的实现软件产品某项功能的工具包。一般以集合kpi和文档、范例、工具的形式出现。

- - -
## fn()()
返回了一个方法，并且在返回的方法内还有一个方法,如果想执行这个放回的方法内的方法。 

那么双括号 `fn()()`；

或者将返回的方法变成一个自执行函数。

- - -
## 不常用的`input` 的type类型

- `file`	定义输入字段和 "浏览"按钮，供文件上传。
- `hidden`	定义隐藏的输入字段。
- `image`	定义图像形式的提交按钮。
- `reset`	定义重置按钮。重置按钮会清除表单中的所有数据。

- - -
## webpack打包问题
`import` `node modules`里的组件或在方法，在`npm run  build` 的时候会一起打包到最终的common.js里吗？

**会**。
- - - 




