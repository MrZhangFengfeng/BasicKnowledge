## 前言

> 在jQuery的`attr`与`prop`提到过在IE9之前版本中如果使用`property`不当会造成内存泄露问题，而且关于`Attribute`和`Property`的
> 区别也让人十分头痛，在HTML5中添加了`data-*`的方式来自定义属性，所谓`data-*`实际上上就是`data-`前缀加上自定义的属性名，
> 使用这样的结构可以进行数据存放。使用`data-*`可以解决自定义属性混乱无管理的现状。

## 读写方式
`data-*`有两种设置方式，可以直接在`HTML`元素标签上书写

    <div id="test" data-age="24">
        Click Here
    </div>

其中的`data-age`就是一种自定义属性，当然我们也可以通过JavaScript来对其进行操作，HTML5中元素都会有一个`dataset`的属性，这是一个
`DOMStringMap`类型的键值对集合。
