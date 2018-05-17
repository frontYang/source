---
title: javascript常用方法收集
date: 2018-03-11 14:41:40
tags: [javascript]
categories: 学习
---


## 一、获取id和ClassName
```js
//获取id
function getId(id) {
    return (typeof id == "object") ? id : document.getElementById(id);
}

//获取ClassName
function getElementsByClassName(name) {
    var tags = document.getElementsByTagName('*') || document.all;
    var els = [];
    for (var i = 0; i < tags.length; i++) {
        if (tags.className) {
            var cs = tags.className.split(' ');
            for (var j = 0; j < cs.length; j++) {
                if (name == cs[j]) {
                    els.push(tags);
                    break
                }
            }
        }
    }
    return els
}

//解决IE8之类不支持getElementsByClassName
if (!document.getElementsByClassName) {
    document.getElementsByClassName = function(className, element) {
        var children = (element || document).getElementsByTagName('*');
        var elements = new Array();
        for (var i = 0; i < children.length; i++) {
            var child = children[i];
            var classNames = child.className.split(' ');
            for (var j = 0; j < classNames.length; j++) {
                if (classNames[j] == className) {
                    elements.push(child);
                    break;
                }
            }
        }
        return elements;
    };
}
```
<!-- more -->

-----
## 二、获取页面常用高度及兼容
```js
// 浏览器窗口可视区域大小（不包括工具栏和滚动条等边线）
var client_w = document.documentElement.clientWidth || document.body.clientWidth;
var client_h = document.documentElement.clientHeight || document.body.clientHeight;

// 网页内容实际宽高（包括工具栏和滚动条等边线）
var scroll_w = document.documentElement.scrollWidth || document.body.scrollWidth;
var scroll_h = document.documentElement.scrollHeight || document.body.scrollHeight;

// 网页内容实际宽高 (不包括工具栏和滚动条等边线）
var offset_w = document.documentElement.offsetWidth || document.body.offsetWidth;
var offset_h = document.documentElement.offsetHeight || document.body.offsetHeight;

// 滚动的高度
var scroll_Top = document.documentElement.scrollTop||document.body.scrollTop;
```
-----

## 三、时间个性化输出
```js
//时间个性化输出
/*
1、< 60s, 显示为“刚刚”
2、>= 1min && < 60 min, 显示与当前时间差“XX分钟前”
3、>= 60min && < 1day, 显示与当前时间差“今天 XX:XX”
4、>= 1day && < 1year, 显示日期“XX月XX日 XX:XX”
5、>= 1year, 显示具体日期“XXXX年XX月XX日 XX:XX”
*/
function timeFormat(time){
    var date = new Date(time),
        curDate = new Date(),
        year = date.getFullYear(),
        month = date.getMonth() + 10,
        day = date.getDate(),
        hour = date.getHours(),
        minute = date.getMinutes(),
        curYear = curDate.getFullYear(),
        curHour = curDate.getHours(),
        timeStr;

    if(year < curYear){
        timeStr = year +'年'+ month +'月'+ day +'日 '+ hour +':'+ minute;
    }else{
        var pastTime = curDate - date,
            pastH = pastTime/3600000;

        if(pastH > curHour){
              timeStr = month +'月'+ day +'日 '+ hour +':'+ minute;
        }else if(pastH >= 1){
              timeStr = '今天 ' + hour +':'+ minute +'分';
        }else{
              var pastM = curDate.getMinutes() - minute;
              if(pastM > 1){
                timeStr = pastM +'分钟前';
              }else{
                timeStr = '刚刚';
              }
        }
    }
    return timeStr;
}
```

-----

## 四、返回顶部的通用方法
```js
//返回顶部的通用方法
function backTop(btnId) {
    var btn = document.getElementById(btnId);
    var d = document.documentElement;
    var b = document.body;
    window.onscroll = set;
    btn.style.display = 'none';
    btn.onclick = function() {
        btn.style.display = 'none';
        window.onscroll = null;
        this.timer = setInterval(function() {
            d.scrollTop -= Math.ceil((d.scrollTop + b.scrollTop) * 0.1);
            b.scrollTop -= Math.ceil((d.scrollTop + b.scrollTop) * 0.1);
            if ((d.scrollTop + b.scrollTop) == 0) clearInterval(btn.timer, window.onscroll = set);
            }, 10);
    };
    function set() {
        btn.style.display = (d.scrollTop + b.scrollTop > 100) ? 'block': 'none'
    }
};
backTop('goTop');
```
----

## 五、事件委托

```html
//关于编写性能高效的javascript事件的技术
<input type="button" id="btn" name="btn" value="BUTTON"/>
<a href="#" id="aa">aa</a>

<script>
document.addEventListener("click",function(evt){
         var target = evt.target;
         switch(target.id){
                   case "btn":
                            alert("button");
                            break;
                   case "aa":
                            alert("a");
                            break;
         }
},false);
</script>
```
----
## 六、倒计时
```js
/**
 * 活动倒计时
 * @param  {string} expiry endtine
 * @param  {object} obj    element
 */
function getTime(expiry,obj) {
    if(expiry == "" || !obj){return;}
    var now = new Date();
    var expiry = expiry;

    var a = now.getFullYear() + '.' + (now.getMonth()+1) + '.' + now.getDate() +' ' + now.getHours() + ':' + now.getMinutes() + ':' + now.getSeconds();
    var b = expiry;

    var startTime = new Date(a);
    var endTime = new Date(b);

    if ( endTime <= now) {
        obj.innerHTML = "距离活动开始还有<i class='s-blue'>00</i>天<i class='s-blue'>00</i>时<i class='s-blue'>00</i>分<i class='s-blue'>00</i>秒";

    } else {
        var days = (endTime - now) / 1000 / 60 / 60 / 24;
        var daysRound =    checkTime(Math.floor(days));
        var hours = (endTime - now) / 1000 / 60 / 60 - (24 * daysRound);
        var hoursRound = checkTime(Math.floor(hours));
        var minutes = (endTime - now) / 1000 / 60 - (24 * 60 * daysRound) - (60 * hoursRound);
        var minutesRound = checkTime(Math.floor(minutes));
        var seconds = (endTime - now) / 1000 - (24 * 60 * 60 * daysRound) - (60 * 60 * hoursRound) - (60 * minutesRound);
        var secondsRound = checkTime(Math.round(seconds));
        obj.innerHTML = '距离活动开始还有:<i class="s-blue">'+ daysRound +'</i>天<i class="s-blue">'+ hoursRound +'</i>时<i class="s-blue">'+ minutesRound +'</i>分<i class="s-blue">'+ secondsRound +'</i>秒';

        function _getTime(expiry){
            return function(){
                getTime(expiry,obj)
            }
        }
        newtime = window.setTimeout(_getTime(expiry,obj), 1000);
    }
}
```
----

## 七、常用正则表达式
- 正整数: `/^[0-9]*[1-9][0-9]*$/`
- 负整数: `/^-[0-9]*[1-9][0-9]*$/`
- 正浮点数: `/^(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*))$/`
- 负浮点数: `/^(-(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*)))$/`
- 浮点数: `/^(-?\d+)(\.\d+)?$/`
- email地址: `/^[\w-]+(\.[\w-]+)*@[\w-]+(\.[\w-]+)+$/`
- url地址: `/^[a-zA-z]+://(\w+(-\w+)*)(\.(\w+(-\w+)*))*(\?\S*)?$/`
- 年/月/日（年-月-日、年.月.日）:`/^(19|20)\d\d[- /.](0[1-9]|1[012])[- /.](0[1-9]|[12][0-9]|3[01])$/`
- 匹配中文字符: `/[\u4e00-\u9fa5]/`
- 匹配空白行: `/\n\s*\r/`
- 匹配中国邮政编码: `/[1-9]\d{5}(?!\d)/`
- 匹配身份证(只是对格式进行检验)：`/\d{15}|\d{18}/`
- 匹配国内电话号码：`/(\d{3}-|\d{4}-)?(\d{8}|\d{7})?/`
- 匹配首尾空白字符: `/^\s*|\s*$/`
- 匹配HTML标记：`/< (\S*?)[^>]*>.*?|< .*? />/`
- 腾讯 QQ 号: `/^[1-9]*[1-9][0-9]*$/`
- 中文、英文、数字及下划线: `/^[\u4e00-\u9fa5_a-zA-Z0-9]+$/`

---
## 八、增删class
```js
//判断是否有这个class
function hasClass( elements,cName ){
    return !!elements.className.match( new RegExp( "(\\s|^)" + cName + "(\\s|$)") );
};
//添加class
function addClass( elements,cName ){
    if( !hasClass( elements,cName ) ){
        elements.className += " " + cName;
    };
};
//移除class
function removeClass( elements,cName ){
    if( hasClass( elements,cName ) ){
        elements.className = elements.className.replace( new RegExp( "(\\s|^)" + cName + "(\\s|$)" ), " " );
    };
};
```
---
## 九、封装cookie组件
```js
var Cookie = {
    // 读取
     get: function(name){
        var cookieStr = "; "+document.cookie+"; ";
        var index = cookieStr.indexOf("; "+name+"=");
        if (index!=-1){
            var s = cookieStr.substring(index+name.length+3,cookieStr.length);
            return unescape(s.substring(0, s.indexOf("; ")));
        }else{
            return null;
        }
    },
    // 设置
     set : function(name,value,expires){
        var expDays = expires*24*60*60*1000;
        var expDate = new Date();
        expDate.setTime(expDate.getTime()+expDays);
        var expString = expires ? "expires="+expDate.toGMTString() : "";
        var pathString = ";path=/";
        document.cookie = name + "=" + escape(value) + expString + pathString;
    },
    // 删除
     del : function(name){
        var exp = new Date(new Date().getTime()-1);
        var s=this.read(name);
        if(s!=null) {
            document.cookie= name + "="+s+"expires="+exp.toGMTString()+";path=/"
        }
    }
};
// demo:
Cookie.set("xxx", "xx", 7);
alert(Cookie.get("xxx"));
Cookie.del("xxx");
```
---
## 十、类型转换函数
```js
var Converter = {
    toDate: function(strDate){
        var sDate = strDate.replace(/(^\s+|\s+$)/g,''); //去两边空格;
        if(sDate==''){
            return null;
        }

        var s = sDate.replace(/[\d]{4,4}[\-/]{1}[\d]{1,2}[\-/]{1}[\d]{1,2}/g, '');
        if (s == '')
        {
            var t=new Date(sDate.replace(/\-/g,'/'));
            var ar = sDate.split(/[-/:]/);
            if(ar[0] == t.getFullYear() && ar[1] == t.getMonth() + 1 && ar[2] == t.getDate())
            {
                return t;   //返回转化成功的日期对象
            }
        }

        return null;

    }
};

console.log(Converter.toDate("2014/9/2"));
console.log(Converter.toDate("2014-9-2"));
console.log(Converter.toDate("2014-09-02"));
// Tue Sep 02 2014 00:00:00 GMT+0800 (中国标准时间)
```
---
## 十一、动态脚本元素
```js
/**
 * 加载JS文件
 * @param  {string}   url      资源url
 * @param  {function} callback 回调函数
 * @param  {string}   charset  资源编码
 */
function loadScript(url, callback, charset) {
    var oscript = document.createElement('script');
    if(charset) oscript.charset = charset;
    oscript.src = url;
    oscript.onload = oscript.onreadystatechange = function() {
        if (!this.readyState || this.readyState == 'loaded' || this.readyState == 'complete') {
            oscript.onload = oscript.onreadystatechange = null;
            callback && callback();
            oscript.parentNode.removeChild(oscript);
        }
    };
    document.body.insertBefore(oscript, document.body.firstChild);
}
```

---
## 十二、js数组去重

```js
var Unique = {
    /**
     * 双重循环去重（经过测试超过两个重复的会留下两个）
     * @param arr
     * @returns {*}
     */
    dbloop: function (arr) {
        var i,
            j,
            res = [];
        for (i = 0; i < arr.length; i++) {
            for (j = i + 1; j < arr.length; j++) {
                if (arr[i] === arr[j]) {
                    arr.splice(i, 1);//当出现相同的元素时，删除重复的元素
                }
            }
        }

        return arr;
    },

    /**
     * 哈希表形式
     * @param arr
     * @returns {Array}
     */
    hash: function (arr) {
        var i,
            hash = {},
            res = [];

        //查询hash对象是否存在当前元素(属性)
        for (i = 0; i < arr.length; i++) {
            if (!hash[arr[i]]) {
                res.push(arr[i]);
                hash[arr[i]] = true;
            }
        }

        return res;
    },

    /**
     * 借助indexOf方法
     * @param arr
     * @returns {Array}
     */
    indexOf: function (arr) {
        var i,
            res = [];

        //查询空数组里面是否已经存在这个值，不存在则推入
        for (i = 0; i < arr.length; i++) {
            if (res.indexOf(arr[i]) === -1) {
                res.push(arr[i]);
                console.log(arr[i]);
            }
        }

        return res;
    }
};

```
---
## 十三、删除左右空格
```js
//删除左右空格
function trim(str) {
    return str.replace(/(^\s*)|(\s*$)/g, '');
}
```
---

---
## 十四、判断图片是否加载完

```js
functionloadImage1(url,callback){
  varimg = newImage();
  img.onload = function(){
        //图片加载完成后执行的操作
  }
  img.src = url;
}
```

---

## 十五、图片按某个尺寸等比缩放
```js
function imgRatio(picW,picH,maxWidth,maxHeight){
   var Ratio,
       wRatio = maxWidth / picW,
       hRatio = maxHeight / picH;
   if(wRatio < 1 || hRatio < 1){
     Ratio = Math.min(wRatio, hRatio);
     picW = picW * Ratio;
     picH = picH * Ratio;
   }
 }

```
---

## 十六、遍历document.querySelectorAll()方法返回的结果
```js
// forEach method, could be shipped as part of an Object Literal/Module
var forEach = function (array, callback, scope) {
  for (var i = 0; i < array.length; i++) {
    callback.call(scope, i, array[i]); // passes back stuff we need
  }
};

// 用法:
// optionally change the scope as final parameter too, like ECMA5
var myNodeList = document.querySelectorAll('li');
forEach(myNodeList, function (index, value) {
  console.log(index, value); // passes index + value back!
});
```

---
## 十七、内容向上滚动，有停顿
```js
/**
 * 用于内容向上滚动，有停顿
 * @param  {object} obj   元素
 * @param  {number} speed 滚动速度（滚动一个子元素高度的时间，单位毫秒）
 * @param  {number} delay 停顿时间
 */
function startmarquee(obj, speed, delay) {
    var t;
    var p = false;
    var o = obj;
    var h;
    if (!o || o.children.length < 2) return true;

    o.innerHTML += o.innerHTML;
    o.scrollTop = 0;
    h = o.children[0].clientHeight;

    o.onmouseover = function() {
        p = true;
    };

    o.onmouseout = function() {
        p = false;
    };

    function start() {
        t = setInterval(scrolling, speed / h);
        if (!p) o.scrollTop += 1;
    }

    function scrolling() {
        if (o.scrollTop % h != 0) {
            o.scrollTop += 1;
            if (o.scrollTop >= o.scrollHeight / 2) o.scrollTop = 0;
        } else {
            clearInterval(t);
            setTimeout(start, delay);
        }
    }
    setTimeout(start, delay);
}

```
---
## 十八、事件处理兼容写法
```js
var eventUtil = {
    // event兼容
    getEvent: function(event) {
        return event ? event : window.event;
    },

    // target兼容
    getTarget: function(event) {
        return event.target ? event.target : event.srcelem;
    },

    // 添加事件句柄
    addHandler: function(elem, type, listener) {
        if (elem.addEventListener) {
            elem.addEventListener(type, listener, false);
        } else if (elem.attachEvent) {
            elem.attachEvent('on' + type, listener);
        } else {
            // 在这里由于.与'on'字符串不能链接，只能用 []
            elem['on' + type] = listener;
        }
    },

    // 移除事件句柄
    removeHandler: function(elem, type, listener) {
        if (elem.removeEventListener) {
            elem.removeEventListener(type, listener, false);
        } else if (elem.detachEvent) {
            elem.detachEvent('on' + type, listener);
        } else {
            elem['on' + type] = null;
        }
    },

    // 添加事件代理
    addAgent: function (elem, type, agent, listener) {
        elem.addEventListener(type, function (e) {
            if (e.target.matches(agent)) {
                listener.call(e.target, e); // this 指向 e.target
            }
        });
    },

    // 取消默认行为
    preventDefault: function(event) {
        if (event.preventDefault) {
            event.preventDefault();
        } else {
            event.returnValue = false;
        }
    },

    // 阻止事件冒泡
    stopPropagation: function(event) {
        if (event.stopPropagation) {
            event.stopPropagation();
        } else {
            event.cancelBubble = true;
        }
    }
};
```

---
## 十九、innerText兼容火狐
```js
//跨浏览器获取innerText
function getInnerText(element){
    return (typeof element.textContent == 'string') ? element.textContent : element.innerText;
}

//跨浏览器设置innerText
function setInnerText(element,text){
    if(typeof element.textContent == 'string'){
        element.textContent = text;
    }else{
        element.innerText = text;
    }
}
```

---

## 二十、throttle(节流)和debounce(防抖)
### 1、throttle(节流)
- 应用场景(需要间隔一定时间触发回调来控制函数调用频率)
  - mousemove
  - mousedown/keydown
  - mousemove
  - keyup
  - 监听滚动事件判断是否到页面底部自动加载更多
  ```js
  //函数节流（throttle）
  function throttle(fn, threshhold, scope) {
      threshhold || (threshhold = 250);
      var last, timer;
      return function() {
          var context = scope || this;
          var now = +new Date(),
              args = arguments;

          if (last && now - last + threshhold < 0) {
              clearTimeout(deferTimer);
              timer = setTimeout(function() {
                  last = now;
                  fn.apply(context, args);
              }, threshhold);
          } else {
              last = now;
              fn.apply(context, args);
          }
      }
  }

  //调用方法
  $('body').on('mousemove', throttle(function(event){
      console.log('tick');
  }, 1000));


  ```

### 2、debounce(防抖)
- 应用场景(对于连续的事件响应我们只需要执行一次回调)
  - 每次 resize/scroll 触发统计事件
  - 文本输入的验证（连续输入文字后发送 AJAX 请求进行验证，验证一次就好）
```js
//函数去抖
function debounce(fn, delay){
    var timer = null;
    return function(){
        var context = this, args = arguments;
        clearTimeout(timer);
        timer = setTimeout(function(){
            fn.apply(context, args);
        }, delay)
    }
}

//调用方法
$('input.username').keypress(debounce(function(){
    //do the Ajax request
}, 250))
```

## 二十一、移动端网络状态判断

 ```js
 //网络状态判断
;(function(window){
    navigator.onLine ? online() : errorTip();
    window.addEventListener("online", online, false);
    window.addEventListener("offline", offline, false);

    //重新连接
    function online() {
        //去掉提示
    }
    //连接断开
    function offline() {
        //触发事件
        //显示提示
        errorTip();
    }
    //错误提示
    function errorTip(){
        alert("网络异常，请稍后再试")
    }
}(window));
 ```

## 二十二、判断浏览器是否支持 placeholder
 ```js
function placeholderSuport() {
    return "placeholder" in document.createElement("input")
}
 ```

## 二十三、判断移动端设备(简单)
```js
// 判断设备来源
function deviceType(){
  var ua = navigator.userAgent;
  var agent = ['Android', 'iPhone', 'Symbian05', 'Windows Phone', 'iPad', 'iPod'];

  for (var i = 0; i < agent.length; i++) {
    if (ua.indexOf(agent[i] > 0)) {
      break;
    }
  }
}

// 判断是否是微信
function isWechat(){
  var ua = navigator.userAgent.toLowerCase();

  return ua.match(/MicroMessage/i) == 'micromessager' ? true : false;
}


// 判断是安卓还是iOS
function isAndroid(){
  var ua = navigator.userAgent.toLowerCase();

  return /iphone|ipad|ipod/.test(ua) ? false : true;
}

```
---

## 二十四、解决不支持console.log报错
```js
var console = console || {log: function() {return;}}
```

## 二十五、js类型判断
```js
var a = '';
var type = Object.prototype.toString.call(a)
// Number: [object Number]
// String: [object String]
// Array: [object Array]
// Object: [object Object]
// Boolean: [object Boolean]
// Null: [object Null]
// Undefined: [object Undefined]
// Function: [object Function]
```





