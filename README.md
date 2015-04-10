# javascript-css-common
##常用代码总结  

javascript和css的常用代码总结。在平时工作和学习中，我们会遇到经常使用的片段，如果每次都去网上搜索的话，会浪费很多的时间，因此在这里把常用的代码总结一下！

##目录
  1. [CSS初始化样式reset.css](#reset)
  2. [去除浮动clearfix](#clearfix)
  3. [js操作cookie](#js-cookie)
  4. [css强制换行/自动换行/强制不换行](#word-wrap)
  
####<a id="reset" name="reset">1. CSS初始化样式reset.css</a>  
不同的浏览器对各个标签默认的样式是不一样的，而且有时候我们也不想使用浏览器给出的默认样式，我们就可以用reset.css去掉其默认样式

```css
body, h1, h2, h3, h4, h5, h6, hr, p, blockquote, dl, dt, dd, ul, ol, li, pre, form, fieldset, legend, button, input, textarea, th, td { margin:0; padding:0; }
body, button, input, select, textarea { font:12px/1.5tahoma, arial, \5b8b\4f53; }
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

####<a id="clearfix" name="clearfix">2. 去除浮动clearfix</a>  
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

####<a id="js-cookie" name="js-cookie">3. js操作cookie</a>

```javascript
var  cookie = {
     //写cookies
     setCookie: function (name, value){
         var  Days = 365;
         var  exp =  new  Date();
         exp.setTime(exp.getTime() + Days*24*60*60*1000);
         document.cookie = name +  "=" + escape (value) +  ";expires="  + exp.toGMTString();
     },
     
     //读取cookies
     getCookie: function (name){
         var  arr,reg= new  RegExp( "(^| )" +name+ "=([^;]*)(;|$)" );
         if (arr=document.cookie.match(reg)) 
             return  unescape(arr[2]);
         else 
             return  null ;
     },
     
     //删除cookies
     delCookie: function (name)
     {
         var  exp =  new  Date();
         exp.setTime(exp.getTime() - 1);
         var  cval= cookie.getCookie(name);
         if (cval!= null ) document.cookie= name +  "=" +cval+ ";expires=" +exp.toGMTString();
     }
}
```

####<a id="word-wrap" name="word-wrap">4. css强制换行/自动换行/强制不换行</a>
```css
/* 强制不换行 */
div{
  white-space:nowrap;
}

/* 自动换行 */
div{ 
  word-wrap: break-word; 
  word-break: normal; 
}

/* 强制英文单词断行 */
div{
  word-break:break-all;
}
```