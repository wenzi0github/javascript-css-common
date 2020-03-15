# javascript常用片段

## 1

### 如何获取当前所在周的起始和结束的日期

```javascript
/**
 * 获取当前星期的起始日期和结束日期
 * @param {string} startFormat 周一的时间格式
 * @param {string} endFormat   周日的时间格式
 * @param {number} timestamp   所在周的时间戳，若不传入，则默认使用当前时刻的时间戳
 * @returns {string, string} {startDate, endDate} 返回的数据
 */
export const getWeekStartAndEnd = (
    startFormat: string,
    endFormat: string,
    timestamp?: number
): {
    startDate: string;
    endDate: string;
} => {
    const oneDayTime = 1000 * 3600 * 24;
    const nowDate = timestamp ? new Date(timestamp) : new Date();
    const now = nowDate.getTime();
    const nowDay = nowDate.getDay() === 0 ? 7 : nowDate.getDay();
    const startDate = new Date(now - oneDayTime * (nowDay - 1));
    const endDate = new Date(now + oneDayTime * (7 - nowDay));

    return {
        startDate: formatTime(startDate.getTime(), startFormat),
        endDate: formatTime(endDate.getTime(), endFormat)
    };
};
```

### 对cookie的添加/获取和删除

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

    //删除cookies， name可以为字符串('username')或数组(['username', 'password', ...])
    delCookie: function (name)
    {
        var delItem = function(item){
            var exp =  new  Date();
            exp.setTime(exp.getTime() - 1);
            var cval = cookie.getCookie(item);
            if (cval!== null ) document.cookie= item +  "=" +cval+ ";expires=" +exp.toGMTString();
        }

       if( typeof name === 'string' ){
            delItem( name );
        }else{
            for(var i=0, len=name.length; i<len; i++){
                delItem( name[i] );
            }
        }
    }
}
```

### js字符串翻转

js中没有直接对字符串进行反转的，需要我们先转换成数组，然后使用数组中的`reverse()`方法翻转，最后在把数组拼接回字符串。

```javascript
var str = "abcdefg";
var revs = str.split("").reverse().join("");
console.log(revs);
```

### js产生随机数字

这是利用js里的`Math.random()`产生的。若使用 *1000000 然后再强制转成整型也行；不过使用下面的方式可以更加简洁一些，直接截取随机数的最后6位进行返回：

```javascript
function getRanNum(){
    return (''+Math.random()).slice(-6); // Math.random().toString().slice(-6)
}
```

其实，产生32位的字母和数字混合的字符串也比较简单，先给出一个含有包含所有字符和数字的混合字符串，然后使用`Math.random()`摘取每位上的字符进行拼接，最后能够得到一个32位的随机字符串；或者使用js的md5()进行加密也可以。可以参考本人收藏的md5加密代码【[md5加密](https://github.com/wenzi0github/js-encrypt/blob/master/md5.js)】

### radio-checkbox-select

jquery对radio, checkbox的input标签和select标签的操作  

input[type=radio]的操作：

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

input[type=checkbox]的操作：

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

select标签：

```javascript
// 获取select选中的value值，给select一个id，直接使用`val()`获取就行
$('#province').val()
```

### requestAnimationFrame的兼容性处理

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

### 获取鼠标移动的方向

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

### 扩展String中的format

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

### js产生随机字符串

```javascript
Math.random().toString(36).substr(2);
```

很有意思，研究了一下，基本上toString后的参数规定可以是2-36之间的任意整数，不写的话默认是10（也就是十进制），此时返回的值就是那个随机数。

若是偶数，返回的数值字符串都是短的，若是奇数，则返回的将是一个很大长度的表示值。

若<10 则都是数字组成，>10 才会包含字母。

所以如果想得到一长串的随机字符，则需使用一个 > 10 且是奇数的参数，另外根据长度自行使用slice(2,n)截取！

### 解析url中的参数

用于解析当前URL中带的参数，如[http://www.xiabingbao.com/javascript/2015/01/30/geturl-param/?a=1&b=wenzi](http://www.xiabingbao.com/javascript/2015/01/30/geturl-param/?a=1&b=wenzi)。

```javascript
function parseUrl(search, name){
    var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", "i");
    var r = url.substr(1).match(reg);
    if (r != null) return unescape(r[2]); return null;
}
parseUrl(window.location.search, 'id');
```

### 时间格式化

```javascript
//格式化日期
Date.prototype.format = function (fmt) {
    var o = {
        "y+": this.getFullYear(),
        "M+": this.getMonth() + 1,                 //月份
        "d+": this.getDate(),                    //日
        "h+": this.getHours(),                   //小时
        "m+": this.getMinutes(),                 //分
        "s+": this.getSeconds(),                 //秒
        "q+": Math.floor((this.getMonth() + 3) / 3), //季度
        "S+": this.getMilliseconds()             //毫秒
    };
    for (var k in o) {
        if (new RegExp("(" + k + ")").test(fmt)){
        if(k == "y+"){
            fmt = fmt.replace(RegExp.$1, ("" + o[k]).substr(4 - RegExp.$1.length));
        }
        else if(k=="S+"){
            var lens = RegExp.$1.length;
            lens = lens==1?3:lens;
            fmt = fmt.replace(RegExp.$1, ("00" + o[k]).substr(("" + o[k]).length - 1,lens));
        }
        else{
            fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
        }
        }
    }
    return fmt;
}
```

使用：  

```javascript
var date = new Date();
console.log(date.format("yyyy年MM月dd日 hh:mm:ss.S")); //输出: 2016年04月01日 10:41:08.133
console.log(date.format("yyyy-MM-dd hh:mm:ss")); //输出: 2016-04-01 10:41:08
console.log(date.format("yy-MM-dd hh:mm:ss")); //输出: 16-04-01 10:41:08
console.log(date.format("yy-M-d hh:mm:ss")); //输出: 16-4-1 10:41:08
```

### 获取当前月份的第一天和最后一天

通过【[如何获取某一天所在的星期](https://www.xiabingbao.com/post/javascript/week-start-end.html)】进行延展而来，这里是获取所在月份的第一天和最后一天。

> 第2个参数是月份，从0开始，范围是0~11；

```javascript
const date = new Date();
const monthFirstDate = new Date(date.getFullYear(), date.getMonth(), 1);
const monthLastDate = new Date(date.getFullYear(), date.getMonth()+1, 0);

console.log(monthFirstDate, monthLastDate);
```

注意，月份从0开始计算，但是，天数从1开始计算。另外，除了日期的默认值为1，小时、分钟、秒钟和毫秒的默认值都是0。

这些参数如果超出了正常范围，会被自动折算。比如，如果月设为15，就折算为下一年的4月。

### 截断字符串并添加省略号

当字符串过长时，可以按照设定的长度来截取：

```typescript
/**
 * @param {string} str 要截取的字符串
 * @param {number} size 截取的长度
 * @param {string} tail 补充的字符，默认是3个点
 * @return {string} 返回截取后的字符串
*/
const truncate = (str: string, size: number, tail?: string) => {
    function trim(str: string) {
        if (isEmpty(str)) {
            return str;
        }
        return str.replace(/(^\s*)|(\s*$)/g, '');
    }

    function isEmpty(str: any) {
        if (str === undefined || str === '' || str === null) {
            return true;
        }
        return false;
    }

    let nstr = trim(str);

    const arr = Array.from(str);

    let cLen = arr.length;
    let length = size <= 0 ? cLen : size;
    if (length > cLen) return nstr;
    nstr = arr.slice(0, length).join('');
    nstr += tail || '...';

    return nstr;
};
```
