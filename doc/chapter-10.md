# 浮动和定位

## 浮动

　　实质上浮动上实现的效果，通过别的方法也是可以实现的，所以大部分情况下我们都可以不选择使用浮动，除了一种情况：文字围绕图像。

### 浮动元素

　　明确一下几点：

1. 浮动元素会从文档的正常流中删除

2. 浮动元素的外边距不会被合并

3. ~~如果浮动的是非替换元素，必须为其声明`width`属性，否则其元素的宽度趋于0(书上提出的，实际在浏览器中并没有测试出来)~~

### 浮动规则

　　在浮动中，包含块是指其最近的块级祖先元素。并且不论该元素本身是什么，一旦设置为浮动就会生成块级框(因此对一个链接设置浮动，其表现就像一个块级元素一样)，所以对浮动元素设置`display:block`并没有什么用。

浮动规则:

1. 浮动元素左右一定不会超出包含块的左右

2. 以左浮动为例，后出现的浮动元素的左外边届必须是之前出现的左浮动元素的右边界。除非之前后出现的浮动元素的顶端在先出现的浮动元素的底端。

```html
<div class="box box1">
</div>
<p>Immutable data cannot be changed once created,
    leading to much simpler application development,
    no defensive copying, and enabling advanced memoization and change detection techniques with simple logic.
</p>
<div class="box box2">
</div>
<p>Immutable data cannot be changed once created,
    leading to much simpler application development,
    no defensive copying, and enabling advanced memoization and change detection techniques with simple logic.
</p>
<div class="box box3">
</div>
```

```css
.box{
    float: left;
    width: 100px;
    height: 100px;
    background: blue;
}
.box1{
    background: blue;
}
.box2{
    background: red;
}
.box3{
    background: yellow;
}
```

显示效果为:

　　 ![box](https://github.com/MrErHu/CSS-The-Definitive-Guide-Summary/blob/master/asset/image/chapter10/float1.png)

出现在后面的浮动红框左边总是紧靠着之前出现的蓝色框，但是由于黄色框在红色和蓝色框底端下面，所以它紧靠的是包含块。

3. 左浮动元素的右边界不会在右边右浮动的左边界的右边。同理亦然。该规则主要为了防止浮动元素互相重叠。

例如:

```html
<div class="left">左浮动</div>
<div class="right">右浮动</div>
```

```css
        .left{
            float: left;
            width: 500px;
            height: 100px;
            background: red;
        }
        .right{
            float: right;
            width: 500px;
            height: 100px;
            background: blue;
        }
```

可以看出，当窗口不足以在同一行容纳两个浮动元素，第二个图像会被向下浮动，直到其顶端在左浮动图形底端之下。

4. 浮动元素不能比包含块的内顶端更高。并且**如果浮动元素在两个合并外间距之间，这个浮动元素好像在两个元素之间存在一个块级元素** ..........