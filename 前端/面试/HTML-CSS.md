# HTML

### XHTML和HTML

XHTML是当前HTML版的继承者， XHTML规范会更加严格

+ XHTML 元素必须被正确地嵌套， 必须成双成对
+ XHTML 元素必须被关闭
+ 标签名必须用小写字母
+ XHTML 文档必须拥有根元素

### HTML语义化

定义：   用正确的标签做正确的事情

优点：

1. 有利于SEO
2. 提高可读性，后续维护方便
3. 在没有样式的时候也可以显示出合理的布局
4. 方便在不同的设备上进行解析



常用标签： 

```
header， nav， aside， article， section， footer
```



### meta viewport

> <meta> 元素可提供有关页面的元信息（meta-information），比如针对搜索引擎和更新频度的描述和关键词

常用代码： 

```
<meta name="viewport" content="width=device-width, initial-scale=no">
```

属性：

+ width 设置viewport 的宽度，为一个正整数，或字符串"device-width"。
+ initial-scale 设置页面的初始缩放值，为一个数字，可以带小数。1.0代表不缩放
+ minimum-scale 允许用户的最小缩放值，为一个数字，可以带小数。1.0代表不能缩小
+ maximum-scale 允许用户的最大缩放值，为一个数字，可以带小数。1.0代表不能放大
+ height 设置viewport 的高度，这个属性对我们并不重要，很少使用
+ user-scalable 是否允许用户进行缩放，值为"no"或"yes", no 代表不允许，yes代表允许









# CSS

### 盒模型

标准的盒子模型中，包含content，padding， border和margin。

IE和标准盒模型的区别：

```
/* 标准模型 */
box-sizing:content-box;
 /*IE模型*/
box-sizing:border-box;
```

### [css选择器](https://juejin.cn/post/6844903460794531848#heading-1)

### link和import引入标签

建议使用`link`标签，慎用`@import`方式

|   区别    |                       Link标签                       |         @import         |
| :-------: | :--------------------------------------------------: | :---------------------: |
| 从属关系  | HTML语法，可导入样式表，也可以定义RSS、rel连接属性等 | CSS语法，只能导入样式表 |
| 加载顺序  |                   css都会同时加载                    |   页面加载完毕后加载    |
|  兼容性   |                     没有兼容问题                     |    css2.1以上才支持     |
| DOM可控性 |              可以通过js插入link改变样式              |      不可以js导入       |
|   权重    |                        权重大                        |         权重小          |



### [文本超出截断](https://www.zoo.team/article/text-overflow)

```
// 单行文本
.ellipsis-line {
    width: 400px;
    overflow: hidden;
    text-overflow: ellipsis; // 文本溢出显示省略号
    white-space: nowrap; // 文本一行内显示， 不换行
}

// 多行文本
.multi-line {
    width: 400px;
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box; // 必须
    -webkit-line-clamp: 3;  // 截断行数
    -webkit-box-orient: vertical; 
}
```



### transition和animation

|          | 过渡         | 动画                     |
| -------- | ------------ | ------------------------ |
| 事件     | 需要事件触发 | 不需要事件触发，自动进行 |
| 关键帧   | 两个关键帧   | 可以很多个关键帧         |
| 执行次数 | 只执行一次   | 可以无限次触发           |
|          |              |                          |



### css性能优化

1. 避免过度约束
2. 避免后代选择符
3. 避免链式选择符
4. 使用紧凑的语法
5. 避免不必要的命名空间
6. 避免不必要的重复
7. 最好使用表示语义的名字。一个好的类名应该是描述他是什么而不是像什么
8. 避免！important，可以选择其他选择器
9. 尽可能的精简规则，你可以合并不同类里的重复规则

### CSS硬件加速

> 硬件加速是指通过**创建独立的复合图层**，让 **GPU** (而不是渲染速度慢的浏览器)来渲染这个图层，从而提高性能，

CSS 中的以下几个属性能触发硬件加速：

1. transform
2. opacity
3. filter

**注意点**

为了避免2D动画在 开始和结束的时候的`repaint`操作，一般使用 `tranform:translateZ(0)`

过多地开启硬件加速可能会耗费较多的内存

GPU 渲染会影响字体的抗锯齿效果。这是因为 GPU 和 CPU 具有不同的渲染机制，即使最终硬件加速停止了，文本还是会在动画期间显示得很模糊。



### [flex布局](https://www.google.com/search?q=flex%E5%B8%83%E5%B1%80&oq=flex%E5%B8%83%E5%B1%80&aqs=chrome..69i57j69i59l2.2299j0j1&sourceid=chrome&ie=UTF-8)

#### **容器属性：**

 **flex-direction，flex-wrap， flex-flow， justify-content， align-items， align-content**



**justify-content**: flex-start | flex-end | center | space-between | space-around;

+ `space-between`：两端对齐，两端贴着边框，项目之间的间隔都相等。
+ `space-around`：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。



**align-items**: flex-start | flex-end | center | baseline | stretch;

+ stretch： 拉伸充满纵轴
+ 项目的第一行文字的基线对齐。



#### 项目属性：

**order, flex-grow**(默认0，不放大),  **flex-shrink**（默认1，不足会缩小）, **flex-basis, flex, align-self**

**flex属性**是`flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为`0 1 auto`。

**align-self属性**允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。 	



### 清除浮动

#### 什么是浮动元素

元素设置了float，就会脱离普通的文档流，影响周围的其他元素。

+ 块级元素会无视这个浮动的块框，使到自身尽可能与这个浮动元素处于同一行，导致被浮动元素覆盖
+ 若是内联元素，则会尽可能围绕浮动元素
+ 脱离文档流之后，父元素不会被撑开

#### 清除浮动方式

```
 // div1, div2都是浮动元素
 <div class="box">
    <div class="div1">1</div>
    <div class="div2">2</div>
  </div>
```

##### 空div

在浮动元素的后面加一个空div，然后设置clear： both；添加了一个多余标签，不符合

```
.clear {
      clear: both;
      height: 0;
 }
```

##### overflow

在父级元素设置overflow： hidden； 触发BFC清除浮动。

```
.box {
	overflow: auto; 
	zoom: 1; /*zoom: 1; 是在处理兼容性问题 */
}
```

##### 伪元素（推荐）

原理和空div一样，但是没有添加多余标签

```
.box:after {
    content: '.';
    height: 0;
    display: block; 
    clear: both;
}
```





### BFC

BFC（Block Formatting Contexts，块级格式化上下文）**是Web页面中盒模型布局的CSS渲染模式**，指一个独立的渲染区域或者说是一个隔离的独立容器，容器内外互不影响。

#### BFC特性

1. 垂直放置。内部的元素会在垂直方向，从顶部开始一个接一个地放置。 
2. margin叠加。元素垂直方向的距离由margin决定。属于同一个BFC的两个相邻 元素的margin会发生叠加
3. 从最左边开始。每个元素的margin box的左边，与包含块border box的左边(对于从左往右的格式化，否则相反)。即使存在浮动也是如此
4. 不与float box叠加。 
5. 独立容器。BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然。 
6. 计算浮动元素高度。 计算BFC的高度时，浮动元素也参与计算（当BFC内部有浮动时，为了不影响外部元素的布局，**BFC计算高度时会包括浮动元素的高度**）

#### 触发BFC方式

- float 除了none以外的值 	

- overflow 除了visible 以外的值（hidden，auto，scroll ） 
- display (table-cell，table-caption，inline-block, **flex**, inline-flex) 
- 绝对定位，即position值为（absolute，fixed） 

- fieldset元素

#### BFC的作用

1. 清除浮动。 BFC 会根据子元素的情况自适高度，这个特性是对父元素使用 overflow:hidden/auto/scroll就可以清除浮动。
2. 不被浮动元素覆盖。 浮动元素会无视兄弟元素的存在， 覆盖在兄弟元素的上面， 为该兄弟元素创建 BFC 后可以阻止这种情况的出现
3. 防止外边距折叠。标准文档流中，块级标签之间竖直方向的margin会以大的为准，这就是margin的塌陷现象。

### 伪类/伪元素

#### 伪类

伪类有四个 :linked,  :visited, :hover,:active,

> 顺序不可以变化，必须是这样。 简记  LV好

##### CSS3新增伪类

```
p:first-of-type	父元素的首个 <p> 元素的每个 <p> 元素
p:last-of-type	父元素的最后 <p> 元素的每个 <p> 元素
p:only-of-type	父元素唯一的 <p> 元素的每个 <p> 元素
p:only-child	父元素的唯一子元素的每个 <p> 元素。
p:nth-child(2)	父元素的第二个子元素的每个 <p> 元素。
:enabled/:disabled 	控制表单控件的禁用状态。
:checked        单选框或复选框被选中。
```

#### 伪元素

### [CSS图形](https://segmentfault.com/a/1190000002780453)



### 图片居中

#### 背景图片

作为元素背景图片， 让元素居中

#### flex布局

```
.father {
        width:500px;
        height: 500px;
        display:flex;
        justify-content:center;
        align-items:center;
}
```

#### table-cell

```
。father {
        width:500px;
        height: 500px;
        text-align:center;
        display:table-cell;
        vertical-align:middle
}
```

#### 绝对定位

```
    .father{
        width:500px;
        height: 500px;
        position:relative
    }
    img{
        position:absolute;
        top:50%;
        left:50%;
        transform:translate(-50%,-50%)
    }
```

### margin和padding

  margin是用来隔开元素与元素的间距，margin用于布局分开元素使元素与元素互不相干；

  padding用于元素与内容之间的间隔，让内容（文字）与（包裹）元素之间有一段间距





### [Less学习](https://less.bootcss.com/#%E5%8F%98%E9%87%8F%EF%BC%88variables%EF%BC%89)