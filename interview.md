---
title: 遇到过的面试题收集
date: 2018-05-05 21:17:39
tags: [面试]
categories: 面试
---
## 2018/05/05某公司
### 手写一个ajax
<!-- more -->
```js
function ajax(opts){
    //实例化XMLHttpRequest，如果兼容ie7-需要加 ActiveXObject判断
    let createXHR = function(){
        if(typeof XMLHttpRequest != 'undefined'){ // ie7 +
            return new XMLHttpRequest();
        }else if(typeof ActiveXObject != 'undefined'){ ie7 -
            let versions = {
                'MSXML2.XMLHTTP.6.0',
                'MSXML2.XMLHTTP.3.0',
                'MSXML2.XMLHTTP'
            }

            for(let verson of versons){
                try{
                    new ActiveXObject(verson);
                    arguments.callee.activeXString = verson;
                    break;

                }catch{
                    // 跳过
                }
            }

            return new ActiveXObject(arguments.callee.activeXString)；
        }else{
            throw new Error('没有可用的XHR对象')
        }
    }

    // 字符串编码
    let addURLParam = function(data){
        let query = [];
        for (let key in data) {
            query.push(encodeURIComponent(key) + '=' + encodeURIComponent(data[key]));
        }
        return query.join('&');
    }

    let xhr = createXHR();

    //onreadystate事件
    xhr.onreadystate = function(){
        if(xhr.readyState == 4){
            if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
                // 请求成功,获取请求主体
                // xhr.requestText
                opts.success && opts.success(xhr.requestText)
            }else{
                // 请求失败
            }
        }
    }
    xhr.open(opts.type, opts.url, true)
    if(opts.type == 'POST'){
        let query = addURLParam(opts.data)
        xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded')
        xhr.send(query)
    }else{
        xhr.send(null)
    }
}
```
### 作用域
```js
var name = 'aaa'
var obj = {
    name: 'bbb',
    prop: {
        name: 'ccc',
        getName: function(){
            return this.name
        }
    }
}

console.log(obj.prop.getName()) // ccc

var test = obj.prop.getName

console.log(test()) // aaa
```
### 闭包
```js
// 假设有四个按钮
var button = document.getElementsByTagName('button')
for (var i = 0; i < button.length; i++) {
    button[i].addEventListener('click', function(){
        console.log(i)
    })
}

无论点击哪个按钮，最后都是输出4
```
### 选中table tr 偶数行（不知道是css还是js）
- css: `table tr:nth-child(even) > td`
- jq: `$('table tr:even')`
- 原生js
    ```js
    let trs = document.querySelectorAll('#tableid tr')
    for(let i = 0; i < trs.length; i++){
        if(i % 2 == 1){ // 奇数

        }else { // 偶数

        }
    }
    ```

### 外边距叠加
> 当两个或更多垂直外边距相遇时，它们将形成一个外边距，这个外边距的高度等于两个发生叠加的外边距的高度中的较大者。

### get和post的区别
- get
    + 用来获取数据。从服务器获取数据（也可以上传数据，参数就是），效率较高
    + 能够被缓存
    + 不安全。get的所有参数全部包装在URL中，明文显示，且服务器的访问日志会记录
    + 数据量限制：不能的浏览器和服务器不同，一般限制在2-8k之间，常见的是1k以内
- post
    + 用来提交数据。但可以向服务器发送数据和下载数据，效率不如GET
    + 不会被缓存
    + 相对安全。post的url中只有资源路径，不会包含参数，参数封装在二进制的数据体中，服务器也不会记录参数。所有涉及用户隐私的数据都要用POST传输
    + post方法提交的数据比较大，大小靠服务器的设定值限定

### 图片按需怎么实现
- 需要按需加载的img标签，图片的真实地址保存到自定的属性里，如'data-src',src属性用一张1像素透明的图片
- 把某范围内的img标签元素保存到数组里面
- 定义一个方法：遍历数据元素，然后判断某元素的offsetTop是否在滚动的可视范围，如果在，交互图片的属性，然后删除这个元素，那么下次遍历就不存在了
- 给window对象添加scroll与resize事件
- 用户有可能快速拉滚动条，这样会导致事件的频繁触发，所以需要添加setTimeout
- 代码来自：https://www.cnblogs.com/focuslgy/p/3194502.html
```js
class LazyImage {
    constructor(opt) {
      this.init(opt)
    }

    init(opt){
        this.setOptions(opt);
        this.load();
        this.fnLoad = this.bind(this,this.load);
        this.addHandler(window,'scroll',this.fnLoad);
        this.addHandler(window,'resize',this.fnLoad);
    }
    setOptions(opt){
        this.holderSrc = opt.holderSrc || 'data-src';
        this.wrapId = opt.wrapId;
        this.imgList = [];
        this.timer = null;
        var targets = null;
        if (document.querySelectorAll) {
            targets = document.querySelectorAll("#" + this.wrapId + " img")
        } else {
            targets = document.getElementById(this.wrapId).getElementsByTagName("img")
        }
        var n = 0,
            len = targets.length;
        // 把元素存到数组里
        for(;n<len;n++){
            if(targets[n].getAttribute(this.holderSrc)){
                this.imgList.push(targets[n]);
            }
        }
    }
    load(){
        // 全部加装，解除事件
        if(this.imgList.length == 0){
            this.removeHandler(window,'scroll',this.fnLoad);
            this.removeHandler(window,'resize',this.fnLoad);
            return
        }
        var st = document.body.scrollTop || document.documentElement.scrollTop,
            clientH = document.documentElement.clientHeight,
            scrollArea = st + clientH;
        for(var n=0;n<this.imgList.length;n++){
            var offsetTop = this.imgList[n].getBoundingClientRect().top+st,
                imgH = this.imgList[n].clientHeight;
            if( scrollArea>(offsetTop-200)&&(imgH+offsetTop)>st ){
                var _src = this.imgList[n].getAttribute(this.holderSrc);
                this.imgList[n].setAttribute('src',_src);
                this.imgList.splice(n,1);//删除已经加载完的元素
                n--;
            }
        }

    }
    bind(obj,fn){
        return () => {
            if(this.timer) clearTimeout(this.timer);
            this.timer = setTimeout(function(){
                fn.apply(obj,arguments);
            },300);

        }
    }
    addHandler(node,type,fn){
        if(node.addEventListener){
            node.addEventListener(type,fn,false);
        }else if(node.attachEvent){
            node.attachEvent('on'+type,function(){
                fn.apply(node,arguments);
            });
        }else{
            node['on' + type] = fn
        }
    }
    removeHandler(node, type, fn) {
        if (node.addEventListener) {
            node.removeEventListener(type, fn, false);
        } else if(node.attachEvent) {
            node.detachEvent("on" + type, fn);
        }else{
            node['on' + type] = null
        }
    }
}
```

<!-- ### 怎么实现分页 -->

### 实现拖拽的基本思路
- 两种方式
    + 使用html的新特性dragable，但是由于在火狐浏览器上dragable每拖拽一次会打开一个新的标签，尝试阻止默认行为和冒泡都无法解决
    + 使用js监听鼠标三个事件（onmousedown,onmousemove,onmouseup），配合节点操作来实现, 根据鼠标的x,y坐标的变化来改变被拖拽的元素top和left。同时需要判断鼠标的状态是否为按下状态，是否是在可拖拽的元素上按下的
        + 在允许拖拽的节点元素上，使用on来监听mousedown(按下鼠标按钮)事件，获取节点坐标
        + 监听mousemove(鼠标移动)事件，修改节点的坐标，实现节点跟随鼠标的效果
        + 监听mouseup(放开鼠标按钮)事件，更新原节点坐标，拖拽完成。

```
鼠标在元素上按下的时候{
    拖拽状态 = 1
    记录下鼠标的x和y坐标
    记录下元素的x和y坐标
   }
 鼠标在元素上移动的时候{
    如果拖拽状态是0就什么也不做。
    如果拖拽状态是1，那么
    元素y = 现在鼠标y - 原来鼠标y + 原来元素y
    元素x = 现在鼠标x - 原来鼠标x + 原来元素x
    }
 鼠标在任何时候放开的时候{
    拖拽状态 = 0
}
```
<!-- ### get请求过程 -->

### 怎么理解原型和原型链
> 每个对象都会在其内部初始化一个属性，就是prototype(原型),当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么就会去prototype里找这个属性，这个prototype又会有自己的prototype，于是就这样一直找下去，也就是我们平时所说的原型链的概念

> `instance.constructor.prototype = instance._proto_`

> js对象是通过引用来传递的，创建的每一个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变。

> 当我们需要一个属性的时，Javascript引擎会先看当前对象中是否有这个属性， 如果没有，就会查找他的Prototype对象是否有这个属性，如此递推下去，一直检索到 Object 内建对象。

---

## 其他经典面试题(待续。。。)
> 参考自https://juejin.im/entry/5533d3a9e4b078a907047186

### 范围（Scope）

```js
(function(){
    var a = b = 5
})()

console.log(b) // 5
// a是通过var声明的，是这个函数的本地变量
// b没有被声明，属于这个函数的全局变量

(function(){
    'use strict';
    var a = b = 5
})()

console.log(b) // b is not defined
// 在严格模式下，会报出未捕获引用错误：b没有定义
// 在严格模式下，如果这个是预期的行为，则需要明确的引用全局变量

(function(){
    'use strict'
    var a = window.b = 5;
})()

console.log(b) // 5
```

### 创建"原生（native）"方法
在String对象上定义一个repeatify函数。这个函数接受一个整数参数，来明确字符串需要重复几次。这个函数要求字符串重复指定的次数。举个例子：

```js
console.log('hello'.repeatify(3));
// 打印出hellohellohello
```

```
Srting.prototype.repeatify = String.prototype.repeatify || function(times){
    var str = '';

    for(var i = 0; i < items; i++){
        str += this;
    }

    return str;
}
```

### 变量提升（Hoisting）
```js
function test(){
    var a;
    function foo(){
        return 2
    }

    console.log(a)
    console.log(foo())

    a = 1
}

test() // undefined 2

// 变量和函数都被提升了，与以下代码相同
function test(){
    var a;
    function foo(){
        return 2
    }

    console.log(a)
    console.log(foo())

    a = 1
}

test()
```

### this 在javascript中是如何工作的
```js
var fullname = 'John Doe'
var obj = {
    fullname: 'Colin Ihrig',
    prop: {
        fullname: 'Aurelio De Rosa',
        getFullname: function(){
            return this.fullname
        }
    }
}

console.log(obj.prop.getFullname()) // Aurelio De Rosa

var test = obj.prop.getFullname;

console.log(test()) // John Doe


// 在js中，一个函数的语境，也就是this关键字的引用，依赖于函数是符合调用的，不是如何定义的
// 在第一个console.log()调用中， getFullname()是作为obj.prop的函数被调用的。因此，这里的语境指向后者并且函数返回对象的 fullname属性。相反，当 getFullname() 被指定为test的变量，那个语境指向全局对象(window)。因为test相当于设置为全局对象的属性。因为这个原因，函数返回window的一个fullname属性，这在这个案例中是在代码片段中第一行设置的。
```

### call()和apply()
修复上一个问题，让最后一个console.log()打印出Aurelio De Rosa
```js

console.log(test.call(obj.prop))

// or

var test = obj.prop.getFullname();
console.log(test) // Aurelio De Rosa

```
> 
