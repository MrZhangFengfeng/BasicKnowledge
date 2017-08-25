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

    var test = document.getElementById('test');
        test.dataset.name = 'winter';
        
这样就为div添加了一个`data-my`的自定义属性，使用JavaScript操作`dataset`有两个需要注意的地方

- 1. 我们在添加或读取属性的时候需要去掉前缀`data-*`，像上面的例子我们没有使用`test.dataset.data-name = 'winter'`;的形式。
- 2. 如果属性名称中还包含连字符(-)，需要转成驼峰命名方式，但如果在CSS中使用选择器，我们需要使用连字符格式。

            [data-birth-date]
            {
                background-color: #0f0;
                width:100px;
                margin:20px;
            }

            test.dataset.birthDate = '19920225';
            
这样我们通过JavaScript设置了`data-birth-date`自定义属性，在CSS样式表为div添加了一些样式。

- - -
读取的时候也是通过dataset对象，使用` . `来获取属性，同样需要去掉`data-`前缀，连字符需要转化为驼峰命名:

    alert(this.dataset.name + ' ' +this.dataset.birthDate);
    
- - - 
### getAttribute/setAttribute
dataset 和getAttribute/setAttribute之间的区别：

    test.dataset.birthDate = '19890615';
    test.dataset.age = '24';
    test.setAttribute('age', 25);
    test.setAttribute('data-sex', 'male');

    console.log(test.getAttribute('data-age')); //24
    console.log(test.getAttribute('data-birth-date')); //19890516
    console.log(test.dataset.age); //24
    console.log(test.age); //25
    console.log(test.dataset.sex); //male

两者都把属性设置到了attribute上，也就是说`getAttribute/setAttribute`可以操作所有的`dataset`内容，`dataset`内容只是`attribute`的一个
子集，特殊就特殊在命名上了，但是`dataset`内只有带有`data-`前缀的属性（没有age=25那个）。

- - -
那么为什么我们还要用`data-*`呢，一个最大的好处是我们可以把所有自定义属性在`dataset`对象中统一管理,而不至于零零散散了。

### 兼容性
别的不多提，IE已经亮瞎我的眼：
# *Internet Explorer 11+*
