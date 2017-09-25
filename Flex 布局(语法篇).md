> 首先感谢阮一峰老师，本文是阅读阮一峰老师的文章来学习的。
- - -

## 行内元素也可以使用 Flex 布局

> Webkit 内核的浏览器，必须加上-webkit前缀。

    .box{
        display: -webkit-flex; /* Safari */
        display: inline-flex;
    }

- - -
## 注意点

设为 Flex 布局以后，其子元素的`float`、`clear`和`vertical-align`属性将失效。

- - -
## 基本概念
容器默认存在两根轴：

- 水平的主轴（main axis）
- 垂直的交叉轴（cross axis）

主轴的开始位置（与边框的交叉点）叫做`main start`，结束位置叫做`main end`；交叉轴的开始位置叫做`cross start`，结束位置叫做`cross end`。
项目默认沿主轴排列。单个项目占据的主轴空间叫做`main size`，占据的交叉轴空间叫做`cross size`。

![flex基本概念](imgs/flex基本概念.png)

- - -
## 容器的属性
以下6个属性设置在容器上。

- flex-direction：决定主轴的方向（即项目的排列方向） `row | row-reverse | column | column-reverse`

- flex-wrap：如果一条轴线排不下，如何换行 `nowrap | wrap(换行，依次向下) | wrap-reverse(换行，第一行在最下面，向上排列)`

- flex-flow：属性是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`。

- justify-content：定义了项目在主轴上的对齐方式。
    - flex-start（默认值）：左对齐
    - flex-end：右对齐
    - center： 居中
    - space-between：两端对齐，项目之间的间隔都相等。
    - space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
    
![justify-content属性](img/justify-content属性.png)


- align-items：定义项目在交叉轴上如何对齐。(如果主轴是X轴,那么Y就是交叉轴)
    - flex-start：交叉轴的起点对齐。
    - flex-end：交叉轴的终点对齐。
    - center：交叉轴的中点对齐。
    - baseline: 项目的第一行文字的基线对齐。
    - stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

![align-items属性](img/align-items属性.png)

- align-content：定义了多根轴线的对齐方式
    - flex-start：与交叉轴的起点对齐。
    - flex-end：与交叉轴的终点对齐。
    - center：与交叉轴的中点对齐。
    - space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
    - space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
    - stretch（默认值）：轴线占满整个交叉轴。

![align-content属性](img/align-content属性.png)

- - -
# 项目的属性
以下6个属性设置在项目上

- order：定义项目的排列顺序。数值越小，排列越靠前，默认为0。

        .item {
          order: <integer>;
        }
    
![order属性](img/order属性.png)

- flex-grow：定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

        .item {
          flex-grow: <number>; /* default 0 */
        }

![flex-grow属性](img/flex-grow属性.png)
如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1
，则前者占据的剩余空间将比其他项多一倍。

- flex-shrink：定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

        .item {
          flex-shrink: <number>; /* default 1 */
        }
        
![flex-shrink属性](img/flex-shrink属性.png)

- flex-basis：定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余
空间。它的默认值为auto，即项目的本来大小。

    .item {
      flex-basis: <length> | auto; /* default auto */
    }

它可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间。
- flex：属性是`flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为`0 1 auto`。后两个属性可选

        .item {
          flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
        }
 
该属性有两个快捷值：`auto (1 1 auto)` 和 `none (0 0 auto)`。

建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。

- align-self：允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承
父元素的`align-items`属性，如果没有父元素，则等同于`stretch`。

    .item {
      align-self: auto | flex-start | flex-end | center | baseline | stretch;
    }

![align-self属性](img/align-self属性.png)

该属性可能取6个值，除了auto，其他都与align-items属性完全一致。
