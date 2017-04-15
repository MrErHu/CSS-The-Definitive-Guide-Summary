# 基本视觉格式化

## 基本框

### 什么是基本框？

　　 CSS会为每个元素生成一个或多个矩形框，又称为元素框。其中包括：内容区、*内边距、边框、外边距*。(斜体表示可选部分)

　　 ![box](https://github.com/MrErHu/CSS-The-Definitive-Guide-Summary/blob/master/asset/image/chapter7/box.png)

　　 这里需要注意几个地方，首先外边距一般都是透明的，但是内容区的背景会被应用到内边距。并且对于边框来说，如果没有显式的给边框赋值(border-color)，其值会取前景色(color)
并且如果边框是不连续的(例如：dotted),我们可以透过边框观察到内边距。其中外边距可以设置为负值(后面会详细介绍)，其他的值都必须是非负值。

## 块级元素

　　 `width`表示左内边界到右被边界的距离(内容区宽度)
　　
　　 `height`表示从上内边界到下内边界的距离(内容区高度)

### 水平格式化

　　 对于块级元素的`width`属性，表示的仅仅只是内容区的宽度，对于整个基本框的宽度是:

> 元素框width = margin-left + padding-left + border-left + width + border-right + padding-right + margin-right

　　 元素框width实际就是父元素的width的值(块级元素的父元素一般都是块级元素)，上述的七个值仅有`margin-left`、`width`、
`margin-right`三个值可以设置为auto。

#### 关于auto

　　 因为元素框的宽度必须等于父元素的width，我们假设父元素的width为400px,那么当前元素中的
`
margin-left + padding-left + border-left + width + border-right + padding-right + margin-right === 400px
`
　　 如果`margin-left`、`width`、`margin-right`中仅有一个是auto，那么值为auto的项可以根据上面的公式计算出来，例如：

```css
.p{
  margin-left: auto;
  margin-right: 100px;
  width: 100px;
}
```

　　 那么margin-left会被计算为200px(400px-100px-100px)。

　　 考虑一下，如果三者都不是auto，但是三者之和并不是父元素的`width`,那么这种情况被称为**过分受限**,浏览器在处理这种情况下，会将`margin-right`
自动设置为auto(这针对于从左至右类型的西方语言，如果是从右至左的非西方语言，是将`margin-left`设置为auto)，那么情况就等同于第一种情况。

　　 如果三者中不只一个auto，如果其中存在两个值为auto:

1. **假设设置了`margin-left`和`margin-right`为auto,`width`为具体数值**: 会将两个外边距设置为相等的长度，使得元素在其父元素居中显示,
```css
p{
  width: 100px;
  margin-left: auto;
  margin-right: auto;
}
```
上面的代码会使段落居中显示，相比`text-align`的区别是，`text-align`仅仅只能使得块级元素的行内元素居中

2. **假设设置了一个`margin`和`width`为auto,另一个`margin`为具体的数值**: 会将设置了值为auto的`margin`会被减为0。

　　 如果三者都被设置了auto，情况的处理也是非常简单，两个`margin`会被减为0，`width`会按照上述的方式进行相应的计算。

#### 负值的`margin`

　　 前面介绍过，仅仅只有`margin`的值可以为负数，其实即便是`margin`的值为负数，但也仍然满足

> 元素框width = margin-left + padding-left + border-left + width + border-right + padding-right + margin-right

例如假设元素框的宽度为400px,`padding`和`border`值都为0，`margin-left`为0，`margin-right`的值为-50px,因此计算出`width`的值为450px，出现的情况是子元素的width宽度大于父元素的width,
虽然看起来很不符合逻辑，实质上是满足规定的。

#### 替换元素

　　 替换元素的宽度是如果没有显式的赋值(即为auto),是由内容的固有宽度去决定(例如img中图像的宽度)，需要注意的是，如果仅仅只是设置了`width`,`height`也会按照替换元素内容的固有大小进行等比例缩放。

### 垂直格式化

　　 块级元素的默认高度由其元素的高度决定，如果显式地设置高度，高于其内容的高度，其效果类似于有额外的内边距。如果小于其内容的高度，浏览器的行为会由`overflow`决定。

#### 垂直属性

　　 垂直相关的属性有7个:

> `margin-top`、`border-top`、`padding-top`、`height`、`padding-bottom`、`border-bottom`、`margin-bottom`

　　 其中可以设置为auto的属性仅有`height`和`margin-top`、`margin-bottom`。其余值必须是非负的。需要注意的是，与水平属性不同的是，在正常流中设置`margin-top`与`margin-bottom`为auto的效果是会被计算为0。例如：

html:

```html
<div class="wrapper">
    <div class="content">

    </div>
</div>
```

```css
.wrapper{
    margin: 100px;
    width: 400px;
    height: 400px;
    border: 1px solid black;
}
.content{
    background-color: blue;
    width: 200px;
    height: 200px;
    margin: auto;
}
```

实际效果为：

　　 ![box](https://github.com/MrErHu/CSS-The-Definitive-Guide-Summary/blob/master/asset/image/chapter7/margin-auto.png)


#### 百分比高度

　　 如果块级元素的高度被设置成了`auto`，则会有以下两种情况:

1. 如果已经显式地声明了包含块的高度，则被包含块将会按照**包含块的高度**对应的百分比绘制。
2. 如果没有显式地声明包含块的高度，则被包含块的高度会被置为auto。

#### auto高度

　　 如果设置了块级元素其高度为auto，情况有两种：

1. 如果包含的是内联元素，则显示时其高度将恰好足以包含其内容，一般会在段落设置一个边框，并认为没有内边距。这样，下边框刚好在文本最后一行的下面，上边框刚好在文本第一行的上面。
2. 如果包含的块级元素，其**默认高度是最高子元素的上边框到最低块级子元素的下边框的距离**(，因此可能会出现一种情况是子元素的外边距会超出包含这些子元素的元素)。不过，**如果块级元素有上内边距和下内边或者有上边框和下边框，其高度是最高子元素上外边距到最低子元素的下外边距**,例如

```html
<div style="height: auto;background-color: silver">
    <div style="height: 20px;margin-top: 10px;margin-bottom: 10px">
        situation one
    </div>
</div>
<div style="height: auto;background-color: silver;border: 1px solid black">
    <div style="height: 20px;margin-top: 10px;margin-bottom: 10px">
        situation two
    </div>
</div>
```

其效果为:

　　 ![box](https://github.com/MrErHu/CSS-The-Definitive-Guide-Summary/blob/master/asset/image/chapter7/height-auto.png)

#### 合并垂直外边距
　　
　　 垂直格式化外边距另一个重要的方面是垂直相邻外边距的合并，但合并仅限于外边距，内边距和边框是不会被合并的，例如：

```html
<ul>
    <li>first</li>
    <li>two</li>
    <li>three</li>
</ul>
```

```css
li{
    margin-top: 10px;
    margin-bottom: 15px;
}
```

　　 其显示效果为

　　 ![box](https://github.com/MrErHu/CSS-The-Definitive-Guide-Summary/blob/master/asset/image/chapter7/margin-merge.png)

　　 如果存在多个相邻的外边距,也会存在相邻的外边距合并的情况。例如:

```html
<ul>
    <li>first</li>
    <li>two</li>
</ul>
<h3>title</h3>
```

```css
ul{
    margin-bottom: 15px;
}
li{
    margin-top: 10px;
    margin-bottom: 20px;
    background-color: silver;
}
h3 {
    margin-top: 28px;
    background-color: blue;
}
```

显示效果是：

　　 ![box](https://github.com/MrErHu/CSS-The-Definitive-Guide-Summary/blob/master/asset/image/chapter7/margin-merge1.png)

我们发现`h3`的上边距是`28px`。因为相当于合并了`28px`、`20px`、`15px`。

　　 但是，如果代码如下：

```html
<ul>
    <li>first</li>
    <li>two</li>
</ul>
<h3>title</h3>
```

```css
ul{
    margin-bottom: 15px;
    border: 1px solid black;

}
li{
    margin-top: 10px;
    margin-bottom: 20px;
    background-color: silver;
}
h3 {
    margin-top: 28px;
    background-color: blue;
}
```

显示效果为:
　　
　　 ![box](https://github.com/MrErHu/CSS-The-Definitive-Guide-Summary/blob/master/asset/image/chapter7/margin-merge2.png)

我们发现`h3`和最下面的`li`变成了`28px+1px+20px`，原因是根据**如果块级元素有上内边距和下内边或者有上边框和下边框，其高度是最高子元素上外边距到最低子元素的下外边**，`li`的外边距会被包含在`ul`的高度中，那么`h3`的外边距会在`28px`与`20px`中进行合并。

　　