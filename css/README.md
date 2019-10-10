# CSS常用的片段

## 1

### CSS初始化样式 reset.css

不同的浏览器对各个标签默认的样式是不一样的，而且有时候我们也不想使用浏览器给出的默认样式，我们就可以用reset.css去掉其默认样式

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

### 去除浮动clearfix

通常我们在有浮动元素的情况下，会在同级目录下再创建一个`<div style="clear:both;"></div>`；不过这样会增加很多无用的代码。此时我们用`:after`这个伪元素来解决浮动的问题，如果当前层级有浮动元素，那么在其父级添加上clearfix类即可。  

```css
.clearfix:after {
    content: "\00A0";
    display: block;
    visibility: hidden;
    width: 0;
    height: 0;
    clear: both;
    font-size: 0;
    line-height: 0;
    overflow: hidden;
}
.clearfix{zoom:1}
```

### css强制换行/自动换行/强制不换行  

```css
/* 强制不换行 */
div {
    white-space:nowrap;
}

/* 自动换行 */
div {
  word-wrap: break-word;
  word-break: normal;
}

/* 强制英文单词断行 */
div{
  word-break:break-all;
}
```

### table边界的样式  

```css
table { border: 1px solid #000; padding: 0; border-collapse: collapse; table-layout: fixed; margin-top: 10px;}  
table td { height: 30px; border: 1px solid #000; background: #fff; font-size: 15px; padding: 3px 3px 3px 8px; color: #000; width: 160px;}
```

### div上下左右居中  

```css
div{
    position:absolute;
    width:400px;
    height:300px;
    left:50%;
    top:50%;
    margin-left:-200px;
    margin-top:-150px;
}
```

### 图片上下左右居中  

一种常用的方式是把外层的div设置为table-cell；然后让内部的元素上下左右居中。当然也还有一种方式，就是把img当做div，参考6中的代码进行设置。  
CSS代码如下：  

```css
.content{
  width: 400px;
  height: 400px;
  border: 1px solid #ccc;
  text-align: center;
  display:table-cell;
  vertical-align:middle;
}
```

html代码如下：  

```html
<div class="content">
  <img src="./4.jpg" alt="img">
</div>
```

### 标题两边的小横杠

我们经常会遇到这样的UI需求，就是标题两边有两个小横岗，之前是怎么实现的呢？比如用个`border-top`属性，然后再把中间的文字进行绝对定位，同时给这个文字一个背景颜色，把中间的这部分盖住。

现在我们可以使用伪元素来实现！

```html
<div class="title">标题</div>
```

```scss
title{
    color: #e1767c;
    font-size: 0.3rem;
    position: relative;

    &:before, &:after{
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
    &:before{
        margin-left: -1.2rem;
    }
    &:after{
        margin-left: 1.2rem;
    }
}
```
