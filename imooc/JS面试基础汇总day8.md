# JS-WEB-API

## 常说的JS包含两部分
- JS基础语法
- JS-WEB-API(W3C标准，包含了DOM操作，BOM操作等)

- - -
## DOM(Document Object Model)操作
DOM标签可以随意些，只要符合标签规则

      <to>132</to>
      <winter>summer</winter>

> DOM其实就是把HTML从字符串解析成一个浏览器和JS能识别和操作的树状结构。
- - -
## DOM是哪种基本的数据结构？
树结构
- - -
## DOM操作的常用API

      var div1 = document.getElementById('div1');  //div1本质上是一个JS对象
      var divList1 = document.getElementByTagName('div');  //divList1本质上是一个JS对象的集合
      
- - -
## DOM节点的att 和 property 有什么区别
- property: s JS对象的一个属性

      p.nodeName
      p.nodeType
  
 - attr: 标签内的属性 
 
- - -
## NodeType
- 1: Element,元素
- 2: Attr,
- 3: Text,代表元素或属性中的文本内容。

- - -
## DOM 操作

- 创建元素节点：`createElement` , `createTextNode`

- 插入节点：`appendchild` ,`insertBefore`,`insertAfter`

- 删除节点：`removeChild`;（先找到要删除节点的父节点，用父节点删除）；

- 替换节点：`replaceChild`;（先找到要替换的节点的父节点，用父节点替换）；

- - -
## BOM问题
- 如何检测浏览器类型
- 拆解URL各部分

- - -
### navigator &&  screen

      //userAgent 属性是一个只读的字符串，声明了浏览器用于 HTTP 请求的用户代理头的值。
      var ua = navigator.userAgent;
      var isChrome = ua.indexOf('Chrome')
      
      console.log(screen.width , screen.height)

### history && location
      history.back();
      history.forword();
      
- - -


