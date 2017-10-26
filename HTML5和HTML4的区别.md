# HTML5和HTML4的区别

## HTML5推出的理由
- HTML4浏览器兼容性差
- 文档结构不够明确
- 对Web应用程序的贡献很小，比如：不允许同时上传多个文件。

- - -
## HTML5语法的改变
- 内容类型不变：HTML5的文件扩展符（html或.htm）与内容类型（text/html）保持不变。
- DOCTYPE声明变化：HTML5只使用<!DOCTYPE html>即可。
- 指定字符编码变化
    - HTML4：`<meta http-equiv=‶content-type″ content=‶text/html; charset=UTF-8″>`
    - HTML5: `<meta charset=‶UTF-8″>`
- 具有boolean值的属性调整
    - 不指定属性值、属性名设定为属性值、字符串设为空时表示属性值为true；
    - 不写该属性表示属性值为false。  
    -  `<input type=‶checkbox″ checked>    表示checked值为true`
    -  `<input type=‶checkbox″ checked=‶checked″>    表示checked值为true`
    -  `<input type=‶checkbox″ checked=‶″>     表示checked值为true`
    -  `<input type=‶checkbox″>         表示checked值为false`

- 可省略引号：HTML5可省略指定属性值时的引号。

- - -
## 新增的元素
- 结构元素：`section`,`article`,`aside`,`figure(表示一段独立的流内容，一般表示主体流内容的一个独立单元)`,等等
- 其他元素：`video`,`audio`,`canvas`等等
- 新增的input元素的类型

- - -
## 废除的元素
- 能使用css代替的元素：`basefont`,`big`,`center` 等
- 不再使用frame框架: 由于frame框架对网页可用性存在负面影响，HTML5中已不支持frame框架，
只支持iframe框架或者用服务器方式创建的由多个页面组成的复合页面的形式
- 废除一些只有部分浏览器支持的元素

- - -
## 全局属性
> HTML5中新增全局属性的概念，全局属性指可以对任何元素都使用的属性。

- `contentEditable属性`: 允许用户编辑元素中内容，使用该属性的元素必须为可以获得鼠标焦点的元素，
而且在点击鼠标后向用户提供一个插入符号，提示用户该元素允许进行编辑。

- `designMode属性`: 用来指定整个页面是否可编辑，当页面可编辑时，页面中所有支持contentEditable属性的元素都变为可编辑状况。
`designMode`属性只能在JavaScript脚本中被修改、编辑。属性值可取on（可编辑）或off（不可编辑）。

- `hidden属性`: HTML5中所有元素都允许使用hidden属性，该属性类似于input元素中hidden元素，boolean值，可设为true（不可见）、false（可见）。
当某元素的hidden属性值为true时，浏览器不渲染该元素，使该元素处于不可见状态，但浏览器创建该元素内容，
即页面加载后允许使用JavaScript脚本将该属性值取消，使该元素可见。

- `spellcheck属性`: 针对input（type=text）与textarea这两个文本输入框提供的一个新属性，
主要对用户输入内容进行拼写与语法检查。属性值为boolean值，可取true或false。

- `tableindex属性`:  当点击Tab键时，让窗口或页面中可获得焦点的链接元素或表单元素进行遍历，tableindex表示该元素第几个被访问到。
若tableindex值为"-1"时表示无法获取该元素.














