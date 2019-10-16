# CSS 常用的片段

## 1

### CSS 初始化样式 reset.css

不同的浏览器对各个标签默认的样式是不一样的，而且有时候我们也不想使用浏览器给出的默认样式，我们就可以用 reset.css 去掉其默认样式

```css
body, h1, h2, h3, h4, h5, h6, hr, p, blockquote, dl, dt, dd, ul, ol, li, pre, form, fieldset, legend, button, input, textarea, th, td { margin:0; padding:0; }
body, button, input, select, textarea { font:12px/1.5 tahoma, arial, \5b8b\4f53; }
h1, h2, h3, h4, h5, h6{ font-size:100%; }
address, cite, dfn, em, var { font-style:normal; }
code, kbd, pre, samp { font-family:couriernew, courier, monospace; }
small{ font-size:12px; }
ul, ol { list-style:none; }
a { text-decoration:none; }
a:hover { text-decoration:underline; }
sup { vertical-align:text-top; }
sub{ vertical-align:text-bottom; }
legend { color:#000; }
fieldset, img { border:0; }
button, input, select, textarea { font-size:100%; }
table { border-collapse:collapse; border-spacing:0; }
```

### 去除浮动 clearfix

通常我们在有浮动元素的情况下，会在同级目录下再创建一个`<div style="clear:both;"></div>`；不过这样会增加很多无用的代码。此时我们用`:after`这个伪元素来解决浮动的问题，如果当前层级有浮动元素，那么在其父级添加上 clearfix 类即可。

```css
.clearfix:after {
    content: '\00A0';
    display: block;
    visibility: hidden;
    width: 0;
    height: 0;
    clear: both;
    font-size: 0;
    line-height: 0;
    overflow: hidden;
}
.clearfix {
    zoom: 1;
}
```

### css 强制换行/自动换行/强制不换行

```css
/* 强制不换行 */
div {
    white-space: nowrap;
}

/* 自动换行 */
div {
    word-wrap: break-word;
    word-break: normal;
}

/* 强制英文单词断行 */
div {
    word-break: break-all;
}
```

### table 边界的样式

```css
table {
    border: 1px solid #000;
    padding: 0;
    border-collapse: collapse;
    table-layout: fixed;
    margin-top: 10px;
}
table td {
    height: 30px;
    border: 1px solid #000;
    background: #fff;
    font-size: 15px;
    padding: 3px 3px 3px 8px;
    color: #000;
    width: 160px;
}
```

### div 上下左右居中

相关文章：[https://www.xiabingbao.com/post/css/css-center.html](https://www.xiabingbao.com/post/css/css-center.html)

文字与元素居中的方式。

#### 绝对定位与 margin

当我们提前知道要居中元素的长度和宽度时，可以使用这种方式：

```css
.container {
    position: relative;
    width: 300px;
    height: 200px;
    border: 1px solid #333333;
}
.content {
    background-color: #ccc;
    width: 160px;
    height: 100px;
    position: absolute;
    top: 50%;
    left: 50%;
    margin-left: -80px; /* 宽度的一半 */
    margin-top: -50px; /* 高度的一半 */
}
```

#### 绝对定位与 transform
当要居中的元素不定宽和定高时，我们可以使用transform来让元素进行偏移。

```css
.container {
    position: relative;
    width: 300px;
    height: 200px;
    border: 1px solid #333333;
}
.content {
    background-color: #ccc;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate3d(-50%, -50%, 0);
    text-align: center;
}
```

#### line-height

`line-height`其实是行高，我们可以用行高来调整布局！

不过这个方案有一个比较大的缺点是：文案必须是单行的，多行的话，设置的行高就会有问题。

```css
.container {
    width: 300px;
    height: 200px;
    border: 1px solid #333333;
}
.content {
    line-height: 200px;
}
```

#### table 布局

给容器元素设置`display: table`，当前元素设置`display: table-cell`：

```css
.container {
    width: 300px;
    height: 200px;
    border: 1px solid #333333;
    display: table;
}
.content {
    display: table-cell;
    vertical-align: middle;
    text-align: center;
}
```

#### flex 布局

我们可以给父级元素设置为`display: flex`，利用 flex 中的`align-items`和`justify-content`设置垂直方向和水平方向的居中。这种方式也不限制中间元素的宽度和高度。

同时，flex 布局也能替换`line-height`方案在某些 Android 机型中文字不居中的问题。

```css
.container {
    width: 300px;
    height: 200px;
    border: 1px solid #333333;
    display: flex;
    align-items: center;
    justify-content: center;
}
.content {
    background-color: #ccc;
    text-align: center;
}
```

### 图片上下左右居中

一种常用的方式是把外层的 div 设置为 table-cell；然后让内部的元素上下左右居中。当然也还有一种方式，就是把 img 当做 div，参考 6 中的代码进行设置。  
CSS 代码如下：

```css
.content {
    width: 400px;
    height: 400px;
    border: 1px solid #ccc;
    text-align: center;
    display: table-cell;
    vertical-align: middle;
}
```

html 代码如下：

```html
<div class="content">
    <img src="./4.jpg" alt="img" />
</div>
```

### 标题两边的小横杠

我们经常会遇到这样的 UI 需求，就是标题两边有两个小横岗，之前是怎么实现的呢？比如用个`border-top`属性，然后再把中间的文字进行绝对定位，同时给这个文字一个背景颜色，把中间的这部分盖住。

现在我们可以使用伪元素来实现！

```html
<div class="title">标题</div>
```

```scss
title {
    color: #e1767c;
    font-size: 0.3rem;
    position: relative;

    &:before,
    &:after {
        content: '';
        position: absolute;
        display: block;
        left: 50%;
        top: 50%;
        -webkit-transform: translate3d(-50%, -50%, 0);
        transform: translate3d(-50%, -50%, 0);
        border-top: 0.02rem solid #e1767c;
        width: 0.4rem;
    }
    &:before {
        margin-left: -1.2rem;
    }
    &:after {
        margin-left: 1.2rem;
    }
}
```
