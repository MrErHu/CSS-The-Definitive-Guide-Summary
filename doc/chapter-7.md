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
```
p{
  width: 100px;
  margin-left: auto;
  margin-right: auto;
}
```
上面的代码会使段落居中显示，相比`text-align`的区别是，`text-align`仅仅只能使得块级元素的行内元素居中