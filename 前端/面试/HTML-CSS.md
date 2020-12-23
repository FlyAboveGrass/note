# HTML

### HTML语义化

定义： 对于正确的内容，选择正确的标签



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

### link和import引入标签

建议使用`link`标签，慎用`@import`方式

|   区别    |                       Link标签                       |         @import         |
| :-------: | :--------------------------------------------------: | :---------------------: |
| 从属关系  | HTML语法，可导入样式表，也可以定义RSS、rel连接属性等 | CSS语法，只能导入样式表 |
| 加载顺序  |                   css都会同时加载                    |   页面加载完毕后加载    |
|  兼容性   |                     没有兼容问题                     |    css2.1以上才支持     |
| DOM可控性 |              可以通过js插入link改变样式              |      不可以js导入       |
|   权重    |                        权重大                        |         权重小          |

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

#### 清除浮动的方式

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

##### 伪元素法（推荐）

原理和空div一样，但是没有添加多余标签

```
.box:after {
    content: '.';
    height: 0;
    display: block; 
    clear: both;
}
```





### 没看： BFC（Block Formatting Contexts，块级格式化上下文）





