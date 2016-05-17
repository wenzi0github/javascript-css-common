# javascript-css-common
##常用代码总结  

javascript和css的常用代码总结。在平时工作和学习中，我们会遇到经常使用的片段，如果每次都去网上搜索的话，会浪费很多的时间，因此在这里把常用的代码总结一下！

##目录
  1. [CSS初始化样式reset.css](#reset)
  2. [去除浮动clearfix](#clearfix)
  3. [js操作cookie](#js-cookie)
  4. [css强制换行/自动换行/强制不换行](#word-wrap)
  5. [table边界的样式](#table-border)
  6. [div上下左右居中](#div-center)
  7. [图片上下左右居中](#img-center)
  8. [js字符串翻转](#js-str-reverse)
  9. [iPad页面适配框架](#ipad_adap)
  10. [google html5.js](#google_html5_js)
  11. [js产生随机数字](#js_random_num)
  12. [table中的td对齐属性](#table-td-align)
  13. [radio-checkbox-select](#radio-checkbox-select)
  14. [requestAnimationFrame的兼容性处理](#requestAnimationFrame)
  15. [获取鼠标移动的方向](#mouse-enter-leave)
  16. [扩展String中的format](#js-string-format)
  17. [html字段转换函数](#html_escape)
  18. [js产生随机字符串](#js_random_string)
  19. [检测浏览器是否支持fixed](#is_support_fixed)
  20. [解析url中的参数](#parse_url_param)
  21. [图片懒加载](#lazyload_img)
  
####<a id="reset" name="reset">1. CSS初始化样式reset.css</a>  
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
        var Days = 365;
        var exp =  new  Date();
        exp.setTime(exp.getTime() + Days*24*60*60*1000);
        document.cookie = name +  "=" + escape (value) +  ";expires="  + exp.toGMTString();
    },
     
    //读取cookies
    getCookie: function (name){
        var arr,reg= new  RegExp( "(^| )" +name+ "=([^;]*)(;|$)" );
        if (arr=document.cookie.match(reg)) 
            return unescape(arr[2]);
        else 
            return null ;
    },
     
    //删除cookies
    delCookie: function (name)
    {
        var exp =  new  Date();
        exp.setTime(exp.getTime() - 1);
        var cval = cookie.getCookie(name);
        if (cval!== null ) document.cookie= name +  "=" +cval+ ";expires=" +exp.toGMTString();
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

####<a id="table-border" name="table-border">5. table边界的样式</a>
```css
table { border :  1px  solid  #000 ; padding : 0 ; border-collapse :  collapse ; table-layout : fixed ;  margin-top : 10px ;}  
table td { height : 30px ; border :  1px  solid  #000 ;  background :  #fff ;   font-size : 15px ;  font-family :Microsoft YaHei;  padding :  3px  3px  3px  8px ; color :  #000 ;  width : 160px ;}
```

####<a id="div-center" name="div-center">6. div上下左右居中</a>  
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

####<a id="img-center" name="img-center">7. 图片上下左右居中</a>  
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

####<a id="js-str-reverse" name="js-str-reverse">8. js字符串翻转</a>  
js中没有直接对字符串进行反转的，需要我们先转换成数组，然后使用数组中的`reverse()`方法翻转，最后在把数组拼接回字符串。  
```javascript
var str = "abcdefg";
var revs = str.split("").reverse().join("");
console.log(revs);
```

####<a id="ipad_adap" name="ipad_adap">9. iPad页面适配框架</a>  
这是一个适配iPad页面的大致框架，包括竖屏和横屏
```css
iPad 适配
/* ipad 竖屏 */
@media only screen 
and (min-device-width : 768px) 
and (max-device-width : 1024px) 
and (orientation : portrait) { 
    body{ color:#000; }
    /* … */
}

/* ipad 横屏 */
@media only screen 
and (min-device-width : 768px) 
and (max-device-width : 1024px) 
and (orientation : landscape) {
    body{ color:#000; }
    /* … */
}
```

####<a id="google_html5_js" name="google_html5_js">10. google html5.js</a>
这是Google提供的js框架，使IE8及以下的浏览器支持html5新标签
[html5.js 链接](http://html5shim.googlecode.com/svn/trunk/html5.js)
```javascript
/*
 HTML5 Shiv v3.7.0 | @afarkas @jdalton @jon_neal @rem | MIT/GPL2 Licensed
*/
(function(l,f){function m(){var a=e.elements;return"string"==typeof a?a.split(" "):a}function i(a){var b=n[a[o]];b||(b={},h++,a[o]=h,n[h]=b);return b}function p(a,b,c){b||(b=f);if(g)return b.createElement(a);c||(c=i(b));b=c.cache[a]?c.cache[a].cloneNode():r.test(a)?(c.cache[a]=c.createElem(a)).cloneNode():c.createElem(a);return b.canHaveChildren&&!s.test(a)?c.frag.appendChild(b):b}function t(a,b){if(!b.cache)b.cache={},b.createElem=a.createElement,b.createFrag=a.createDocumentFragment,b.frag=b.createFrag();
a.createElement=function(c){return!e.shivMethods?b.createElem(c):p(c,a,b)};a.createDocumentFragment=Function("h,f","return function(){var n=f.cloneNode(),c=n.createElement;h.shivMethods&&("+m().join().replace(/[\w\-]+/g,function(a){b.createElem(a);b.frag.createElement(a);return'c("'+a+'")'})+");return n}")(e,b.frag)}function q(a){a||(a=f);var b=i(a);if(e.shivCSS&&!j&&!b.hasCSS){var c,d=a;c=d.createElement("p");d=d.getElementsByTagName("head")[0]||d.documentElement;c.innerHTML="x<style>article,aside,dialog,figcaption,figure,footer,header,hgroup,main,nav,section{display:block}mark{background:#FF0;color:#000}template{display:none}</style>";
c=d.insertBefore(c.lastChild,d.firstChild);b.hasCSS=!!c}g||t(a,b);return a}var k=l.html5||{},s=/^<|^(?:button|map|select|textarea|object|iframe|option|optgroup)$/i,r=/^(?:a|b|code|div|fieldset|h1|h2|h3|h4|h5|h6|i|label|li|ol|p|q|span|strong|style|table|tbody|td|th|tr|ul)$/i,j,o="_html5shiv",h=0,n={},g;(function(){try{var a=f.createElement("a");a.innerHTML="<xyz></xyz>";j="hidden"in a;var b;if(!(b=1==a.childNodes.length)){f.createElement("a");var c=f.createDocumentFragment();b="undefined"==typeof c.cloneNode||
"undefined"==typeof c.createDocumentFragment||"undefined"==typeof c.createElement}g=b}catch(d){g=j=!0}})();var e={elements:k.elements||"abbr article aside audio bdi canvas data datalist details dialog figcaption figure footer header hgroup main mark meter nav output progress section summary template time video",version:"3.7.0",shivCSS:!1!==k.shivCSS,supportsUnknownElements:g,shivMethods:!1!==k.shivMethods,type:"default",shivDocument:q,createElement:p,createDocumentFragment:function(a,b){a||(a=f);
if(g)return a.createDocumentFragment();for(var b=b||i(a),c=b.frag.cloneNode(),d=0,e=m(),h=e.length;d<h;d++)c.createElement(e[d]);return c}};l.html5=e;q(f)})(this,document);
```

####<a id="js_random_num" name="js_random_num">11. js产生随机数字</a>  
这是利用js里的`Math.random()`产生的。若使用 *1000000 然后再强制转成整型也行；不过使用下面的方式可以更加简洁一些，直接截取随机数的最后6位进行返回： 
```javascript
function getRanNum(){
    return (''+Math.random()).slice(-6); // Math.random().toString().slice(-6)
}
```
其实，产生32位的字母和数字混合的字符串也比较简单，先给出一个含有包含所有字符和数字的混合字符串，然后使用`Math.random()`摘取每位上的字符进行拼接，最后能够得到一个32位的随机字符串；或者使用js的md5()进行加密也可以。可以参考本人收藏的md5加密代码【[md5加密](https://github.com/wenzi0github/js-encrypt/blob/master/md5.js)】

####<a id="table-td-align" name="table-td-align">12. table中td的对齐属性</a>  
在table中有两个默认的属性：align(横向对齐属性)和valign(竖向对齐属性)。  
align有三个值：left(左对齐，默认)，center(左右居中)，right(右对齐)；如想要文字居中，可以：  
```html
<td align='center'>wenzi</td>
```
valign也三个值：top(上对齐)，middle(上下居中，默认)，bottom(下对齐)；如想要文字上居中，可以：  
```html
<td valign='top'>wenzi</td>
```

当然，为了实现结构与样式的分离，**推荐**使用CSS的属性。  
```css
td{
  align: center; /* 横向对齐：left, center, right */
  vertical-align: top; /* 竖向对齐：top, middle, bottom */
}
```
####<a id="radio-checkbox-select" name="radio-checkbox-select">13. radio-checkbox-select</a>  
jquery对radio, checkbox的input标签和select标签的操作  

input[type=radio]的操作  
```javascript
// boolean, 判断radio是否有被选中的元素
$('#myradio input[type=radio]').is(':checked');

// 设置radio选中某个元素
$('#myradio input:eq(1)').prop('checked', true);

// 设置radio取消选中某个元素
$('#myradio input:eq(1)').prop('checked', false);

// 获取选中的radio的值
var val = $('#myradio input[type=radio]:checked').val();
```

input[type=checkbox]的操作  
```javascript
// 判断复选框是否选中
var bool = $('#mycheckbox input[type=checkbox]').is(':checked') ;

// 全选，所有的checkbox都添加上checked属性
$('#checkall').click(function(){
    $('#like input[type=checkbox]').prop('checked', true);
})

// 反选，判断当前的checkbox是否被选中，若被选中则设置checked属性为false，否则设置checked属性为true
$('#reverse').click(function(){
    $('#like input[type=checkbox]').each(function(){
        if($(this).is(':checked')){
            $(this).prop('checked', false);
        }else{
            $(this).prop('checked', true);
        }
    })
})

// 取消选中，去掉所有checkbox的checked属性
$('#deleteall').click(function(){
    $('#like input[type=checkbox]').prop('checked', false);
})

// 获取选中的值
$('#getcheckval').click(function(){
    var result = [];
    $('#mycheckbox input[type=checkbox]:checked').each(function(){
        result.push( $(this).val() );
    })
    console.log(result);
})
```
select标签
```javascript
// 获取select选中的value值，给select一个id，直接使用`val()`获取就行
$('#province').val()
```

####<a id="radio-checkbox-select" name="requestAnimationFrame">14. requestAnimationFrame的兼容性处理</a>  
```javascript
// http://paulirish.com/2011/requestanimationframe-for-smart-animating/
// http://my.opera.com/emoller/blog/2011/12/20/requestanimationframe-for-smart-er-animating
// requestAnimationFrame polyfill by Erik Möller. fixes from Paul Irish and Tino Zijdel
// MIT license
(function() {
    var lastTime = 0;
    var vendors = ['ms', 'moz', 'webkit', 'o'];
    for (var x = 0; x < vendors.length && !window.requestAnimationFrame; ++x) {
        window.requestAnimationFrame = window[vendors[x] + 'RequestAnimationFrame'];
        window.cancelAnimationFrame = window[vendors[x] + 'CancelAnimationFrame'] || window[vendors[x] + 'CancelRequestAnimationFrame'];
    }
    if (!window.requestAnimationFrame) window.requestAnimationFrame = function(callback, element) {
        var currTime = new Date().getTime();
        var timeToCall = Math.max(0, 16 - (currTime - lastTime));
        var id = window.setTimeout(function() {
            callback(currTime + timeToCall);
        }, timeToCall);
        lastTime = currTime + timeToCall;
        return id;
    };
    if (!window.cancelAnimationFrame) window.cancelAnimationFrame = function(id) {
        clearTimeout(id);
    };
}());
```

####<a id="mouse-enter-leave" name="mouse-enter-leave">15. 获取鼠标移动的方向</a>  
我们一定遇见过鼠标从哪个地方进入到某div中，遮罩就从哪个方向出现，鼠标从哪个地方离开这个div，遮罩就从哪个方向消失。整个动画实现的基础就是获取鼠标移动的方向。  

```javascript
/*
 * 获取元素移动的方向
 * @param  $element  元素的jQuery对象
 * @param  event     事件对象
 * @return direction 返回一个数字：0:上，1:右，2:下，3:左
 **/
function getDirection($element, event) {
    var w = $element.width(),
        h = $element.height(),
        x = (event.pageX - $element.offset().left - (w / 2)) * (w > h ? (h / w) : 1),
        y = (event.pageY - $element.offset().top - (h / 2)) * (h > w ? (w / h) : 1),
        direction = Math.round((((Math.atan2(y, x) * (180 / Math.PI)) + 180) / 90) + 3) % 4;

    return direction;
}

$('#content').on('mouseenter', function(event){
    console.log( 'enter: '+ getDirection($(this), event) );
}).on('mouseleave', function(event){
    console.log( 'leave: '+getDirection($(this), event) );
})
```

####<a id="js-string-format" name="js-string-format">16. 扩展String中的format</a>  
* 对String原型进行扩展: String.prototype.methodName=function...
* 正则表达式： /\{(\d+)\}/g ；取"{0}"这种格式的占位符，并对里面的数字放入子组
* js 的 replace 方法有一种重载, string.format(regex , function(group0【匹配项】,group1【子组第一个】...){  //code...  }) ；对于每次匹配到的一个占位符，都从参数相应的位置取得替换项。

```javascript
String.prototype.format = function () {
    var args = arguments;
    var reg = /\{(\d+)\}/g;
    return this.replace(reg, function (g0, g1) {
        return args[+g1] || '';
    });
};
//用法：
"hello {0},your age is {1},so {0}'s age is {1}".format("tom",12);
//"hello tom,your age is 12,so tom's age is 12"
```

若不想在`String`的类型上进行拓展，也可以这样修改：  

```javascript
var tool = {
    format : function(str){
        var args = arguments;
	    var reg = /\{(\d+)\}/g;
	    return str.replace(reg, function (g0, g1) {
	    	g1++;

	        return args[+g1] || '';
	    });
    }
}

tool.format("hello {0},your age is {1},so {0}'s age is {1}", "tom", 12);
// "hello tom,your age is 12,so tom's age is 12"
```


####<a id="html_escape" name="html_escape">17. html字段转换函数</a>  
```javascript
function escapeHTML(text) {  
    var replacements= {"<": "&lt;", ">": "&gt;","&": "&amp;", "\"": "&quot;"};                      
    return text.replace(/[<>&"]/g, function(character) {  
        return replacements[character];  
    }); 
}
```

####<a id="js_random_string" name="js_random_string">18. js产生随机字符串</a>  
```javascript
Math.random().toString(36).substr(2);
```
很有意思，研究了一下，基本上toString后的参数规定可以是2-36之间的任意整数，不写的话默认是10（也就是十进制），此时返回的值就是那个随机数。

若是偶数，返回的数值字符串都是短的，若是奇数，则返回的将是一个很大长度的表示值。
若<10 则都是数字组成，>10 才会包含字母。
所以如果想得到一长串的随机字符，则需使用一个 > 10 且是奇数的参数，另外根据长度自行使用slice(2,n)截取！

####<a id="is_support_fixed" name="is_support_fixed">19. 检测浏览器是否支持fixed</a>  
```javascript
function isSupportFixed() {
    var userAgent = window.navigator.userAgent, 
        ios = userAgent.match(/(iPad|iPhone|iPod)\s+OS\s([\d_\.]+)/),
        ios5below = ios && ios[2] && (parseInt(ios[2].replace(/_/g, '.'), 10) < 5),
        operaMini = /Opera Mini/i.test(userAgent),
        body = document.body,
        div, isFixed;

    div = document.createElement('div');
    div.style.cssText = 'display:none;position:fixed;z-index:100;';
    body.appendChild(div);
    isFixed = window.getComputedStyle(div).position != 'fixed';
    body.removeChild(div);
    div = null;

    return !!(isFixed || ios5below || operaMini);
}
```

####<a id="parse_url_param" name="parse_url_param">20. 解析url中的参数</a>  
用于解析当前URL中带的参数，如 http://www.xiabingbao.com/javascript/2015/01/30/geturl-param/?a=1&b=wenzi 

```javascript
var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", "i");
var r = window.location.search.substr(1).match(reg);
if (r != null) return unescape(r[2]); return null;
```

####<a id="lazyload_img" name="lazyload_img">21. 图片懒加载</a>  
对需要懒加载的图片，把真实的图片地址放到`_src`的属性中，不要写`src`属性，因为src的值为空时也会请求，或者为src设置一个1x1的占位图片。  

把整个页面里的图片划分区域，每个区域按顺序设置图片的`name`属性，值为`page_cnt_{num}`，num从1开始依次递增不能有间断：

```html
<div class="area1">
	<img _src="http://inews.gtimg.com/newsapp_ls/0/301518240_150120/0" name="page_cnt_1" />
	<img _src="http://inews.gtimg.com/newsapp_ls/0/301518240_150120/0" name="page_cnt_1" />
	<img _src="http://inews.gtimg.com/newsapp_ls/0/301518240_150120/0" name="page_cnt_1" />
</div>
<div class="area2">
	<img _src="http://inews.gtimg.com/newsapp_ls/0/301518240_150120/0" name="page_cnt_2" />
	<img _src="http://inews.gtimg.com/newsapp_ls/0/301518240_150120/0" name="page_cnt_2" />
	<img _src="http://inews.gtimg.com/newsapp_ls/0/301518240_150120/0" name="page_cnt_2" />
</div>
```

当滚动条滚动到当前区域时，则把area1区域里name的值是**page_cnt_1**的图片都加载完成，而area2则在滚动条再次滚动到相应的距离时才加载。

```javascript
(function(){var a=this;a.pageSize=1E3;a.pageQuotiety=.5;a.imgName="page_cnt_";a.imgs=[];var e,g;e=window.navigator.userAgent.toLowerCase();g=/msie/.test(e);/gecko/.test(e);/opera/.test(e);/safari/.test(e);var f=function(b,a){var d=a?a:document;return"object"==typeof b?b:d.getElementsByName(b)};a.getWindowSize=function(){var b={};if(window.self&&self.innerWidth)return b.width=self.innerWidth,b.height=self.innerHeight,b;if(document.documentElement&&document.documentElement.clientHeight)return b.width=
document.documentElement.clientWidth,b.height=document.documentElement.clientHeight,b;b.width=document.body.clientWidth;b.height=document.body.clientHeight;return b};a.getObjPosition=function(b){var a={};a.x=b.offsetLeft;for(a.y=b.offsetTop;b=b.offsetParent;)a.x+=b.offsetLeft,a.y+=b.offsetTop;return a};a._getPageScroll=function(){var a;"undefined"!=typeof window.pageYOffset?a=window.pageYOffset:document.documentElement&&document.documentElement.scrollTop?a=document.documentElement.scrollTop:document.body&&
(a=document.body.scrollTop);return a};a._loadImages=function(a){if(a){var c=a;"string"==typeof a&&(c=f(a));for(a=0;a<c.length;a++){var d=c[a];"object"==typeof d&&d.getAttribute("_src")&&(d.setAttribute("src",d.getAttribute("_src")),d.removeAttribute("_src",0))}delete c}};a._loadAllImgs=function(){for(var b=0;a.imgs[b];)a._loadImages(a.imgs[b][0]),b++};a.getImgPosition=function(){for(var b=1,c=f(a.imgName+b);c&&0<c.length;){var c=f("page_cnt_"+b),d=a.getImgLoadPosition(c[0]);a.imgs.push([c,c[0],d]);
b++;c=f(a.imgName+b)}};a.getImgLoadPosition=function(b){var c={imgTop:0,pageTop:0};b&&(a.getWindowSize(),c.imgTop=parseInt(a.getObjPosition(b).y),c.pageTop=parseInt(1E3*(c.imgTop/1E3-a.pageQuotiety)));return c};a._addScrollEven=function(){g?window.attachEvent("onscroll",a._scrollFn):window.addEventListener("scroll",a._scrollFn,!1)};a._removeScrollEven=function(){g?window.detachEvent("onscroll",a._scrollFn):window.removeEventListener("scroll",a._scrollFn,!1)};a._scrollFn=function(){var b=a._getPageScroll(),
c=a.getWindowSize().height;if(0==c)a._loadAllImgs();else for(var d=0,e=0;a.imgs[d];)b+c<a.imgs[d][2].pageTop||(a._loadImages(a.imgs[d][0]),e++),d++,e>=a.imgs.length&&a._removeScrollEven()};a.getImgPosition();a._addScrollEven();a._scrollFn()})();
```
