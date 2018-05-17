---
title: javascript知识点笔记
date: 2017-04-30 15:00:51
tags: javascript
categories: 学习
---

## 一、break、 continue 和 return 的用法
- break：会使运行的程序立刻退出包含在最内层的循环或者退出一个switch语句
栗子1：
```js
for(var i=1;i<=10;i++){
    if(i==6) break;
    document.write(i);
}
//输出结果：12345
```
<!-- more -->
- continue：和break相似，不同的是，它不是退出一个循环，而是开始循环的一次新迭代，continue语句只能用在while语句、do/while语句、for语句、或者for/in语句的循环体内，在其它地方使用都会引起错误！

- return： return语句就是用于指定函数返回的值。return语句只能出现在函数体内，出现在代码中的其他任何地方都会造成语法错误！
---


## 二、instanceof：
- A instanceof B

- A 是一个对象，B 是定义类的构造函数，如果 A 是 B 的实例，返回true，（也就是说，如果 A 继承自 B.prototype，返回true；这里的继承可以不是直接继承，如果 A 所继承的对象继承自另一个对象， B 继承自 B.prototype，返回的结果也为true）;
---
## constructor
- 检测对象是否属于某个类
- 每个javascript函数都自动拥有一个 prototype 属性，这个属性的值是一个对象，这个对象包含唯一一个不可枚举属性 constructor，constructor 属性的值是一个函数对象。
- 新定义的 prototype 不含有 constructor 属性

栗子1：
```js
var F = function(){}
var p = F.prototype; // 这是与F相关联的原型对象
var c = p.constructor; // 这是与原型相关联的函数
c === F; // true, 对于任意函数F.prototype.constructor == F

// 可以看到构造函数的原型中存在预先定义好的constructor 属性，这意味着对象通常继承的constructor均指代它们的构造函数。
```

栗子2：
```js
var o = new F();
o.constructor === F; // true,constructor属性指代这个类
```
---
## 三、hasOwnProperty
- A.hasOwnProperty("B");
- A 对象，B 属性名，如果 B 是 A 的自有属性 (B 不是 A
 的继承属性)，则返回true。

栗子1：
```js
var o = { x: 1 };
o.hasOwnProperty("x"); //true,o 有一个自有属性 x
o.hasOwnProperty("y"); // false, o 中不存在属性 y
o.hasOwnProperty("toString"); //false, toString 是继承属性
```
---
## 四、in
- "A" in B
- A 是属性名（字符串），B 是对象，如果对象的自有属性或继承属性中包含这个属性则返回true
- 可以区分不存在的属性和存在但值为undefined的属性

栗子1：
```js
var o = { x: 1 };
"x" in o; // true  "x"是 o 的属性
"y" in o; // false "y"不是 o 的属性
"toString" in o; // true o 继承 toString 属性
```
---

## 五、call() / apply()
- 改变this的指向； 当一个 对象 没有某个方法，但是其他的有，我们可以借助call或apply用其它对象的方法来操作。

### 1、call()
- 参数长度确定时使用
- A.call(B, arg1, arg2...)

### 2、apply()
- 参数长度不确定时使用
- A.call(B, [arg1, arg2...])

栗子1：（from MDN web docs）调用父构造函数
```js
function Product(name, price){
    this.name = name;
    this.price = price;
}
//===============call==============

function Food(name, price){
    Product.call(this, name, price);
    this.category = 'food';
}

等同于：

function Food(name, price){
    this.name = name;
    this.price = price;
    this.category = 'food';
}


```
栗子2：（from MDN web docs）调用匿名函数
```js
//===============call==============

var animals = [
    {species: 'Lion', name:'King'},
    {species: 'Whale', name:'Fail'}
];

for(var i = 0; i < animals.length; i++){
    (function(i){
        this.print = function(){
            console.log("#" + i + " " + this.species + ": " + this.name);
        }

        this.print();
    }).call(animals[i], i)
}

```
栗子3：（from MDN web docs）调用函数并且指定上下文的'this'

```js
function greet(){
    var reply = [this.person, 'Is An Awesome', this.role].join(' ');
    console.log(reply);
}

var i = {
    person: 'Douglas Crockford', role: 'Javascript Developer'
}

greet.call(i); //Douglas Crockford Is An Awesome Javascript Developer
```

---

## 六、forEach()
- 遍历数组的函数,传递的函数做为forEach的第一个参数，forEach使用三个参数调用该函数：数组元素，元素的索引，数组本身；【注意：forEach无法在所有元素都传递给调用函数的函数之前终止遍历（即没有像for循环中使用的相应的break语句），如果需要终止循环，必须把forEach()放在一个try块中，并能抛出一个异常，如果forEach()调用函数抛出foreach.break异常，循环会提前终止】

```js
data.forEach(function(v, i ,a){
// v:数组元素
// i：元素索引
// a：数组本身
})


```
---

## 七、cloneNode()
- 创建节点的拷贝，并返回该副本；克隆所有属性以及它们的值。
- 语法：var dupNode = node.cloneNode(deep);
- dupNode：要克隆的节点
- node：克隆的新节点 node
- deep(可选)： true==>克隆所有后代;false只克隆指定的节点

栗子：（from W3School）
```html
//html
<ul id="myList1"><li>Coffee</li><li>Tea</li></ul>
<ul id="myList2"><li>Water</li><li>Milk</li></ul>

<script>
// js
var node=document.getElementById("myList2").lastChild.cloneNode(true);
document.getElementById("myList1").appendChild(node);
</script>

// 克隆之前
//myList1:
Coffee
Tea

//myList2:
Water
Milk

// 克隆之后
//myList1:
Coffee
Tea
Milk

//myList2:
Water
Milk

```
---

## 八、cssText
- 批量修改样式，可以尽量避免页面reflow，提高页面性能
- 会把原有的cssText清掉, 所以最好使用 累加 的方法；

```js
element.style.cssText += "width:20px;height:20px;border:solid 1px red;";

// cssText（假如不为空）在IE中最后一个分号会被删掉，可以在前面添加一个分号
Element.style.cssText += ‘;width:100px;height:100px;top:100px;left:100px;’

```
---

## 九、setTimeout() / setInterval()
- 用来注册在指定的时间之后单次或重复调用的函数
- 两者都会返回一个值，可以传递给clearTimeout()用于取消这个函数的执行
- 由于历史原因，第一个参数可以作为字符串传入，如果这么做，那这个字符串会在指定的超时时间或间隔之后进行求值（相当与执行eval()）
- HTML5 规范还允许传入额外的参数（不支持IE）

### 1、setTimeout
- 用来实现一个函数在指定的毫秒数之后运行

### 2、setInterval
- 与setTimeout一样，只是这个函数会在指定毫秒数的间隔里重复调用
```js
setInterval(undateClock, 60000);// 每60s调用一个undateClock
```

栗子：(form JavaScript 权威指南)
```js
function invoke(f, start, interval, end){
    if(!start) start= 0 ; // 默认设置为0 ms
    if(arguments.length <= 2){ // 单次调用模式
        setTimeout(f, start); // start ms 后 单次调用 f()
    }else { // 多次调用模式
        setTimeout(repeat, start); // 在 start 调用repeat()
        function repeat(){ 
            var h = setInterval(f, interval); // 循环调用f()
            if(end) setTimeout(function(){clearInterval(h)}, end);// 在end ms后停止调用，前提是end已经定义
        }
    }
}
```
---
##  十、encodeURIComponent() / decodeURIComponent()
### 1、encodeURIComponent
- 对统一资源标识符（URI）的组成部分进行编码的方法
- 转义除了字母、数字、(、)、.、!、~、*、'、-和_之外的所有字符
栗子：(from MDN web docs)
```js
var fileName = 'my file(2).txt';
var header = "Content-Disposition: attachment; filename*=UTF-8''" 
             + encodeRFC5987ValueChars(fileName);

console.log(header); 
// 输出 "Content-Disposition: attachment; filename*=UTF-8''my%20file%282%29.txt"


function encodeRFC5987ValueChars (str) {
    return encodeURIComponent(str).
        // 注意，仅管 RFC3986 保留 "!"，但 RFC5987 并没有
        // 所以我们并不需要过滤它
        replace(/['()]/g, escape). // i.e., %27 %28 %29
        replace(/\*/g, '%2A').
            // 下面的并不是 RFC5987 中 URI 编码必须的
            // 所以对于 |`^ 这3个字符我们可以稍稍提高一点可读性
            replace(/%(?:7C|60|5E)/g, unescape);
}
```

### 2、decodeURIComponent
- 用于解码由 encodeURIComponent 方法或者其它类似方法编码的部分统一资源标识符（URI）
- 将已编码 URI 中所有能识别的转义序列转换成原字符
栗子：(from MDN web docs)
```js
decodeURIComponent("JavaScript_%D1%88%D0%B5%D0%BB%D0%BB%D1%8B");
// "JavaScript_шеллы"

```
---

## 十一、getBoundingClientRect()
- 返回元素在视口坐标中的位置，返回一个有left、right、top、bottom属性的对象。
- left和top属性表示元素的左上角的X和Y坐标，right和bottom属性表示元素右下角的X和Y坐标
- 在很多浏览器中，还返回 width和height，在是在ie中未实现
- 不是实时的，在用户滚动或改变浏览器窗口大小时不会更新

栗子：（from JavaScript权威指南）
```js
var box = e.getBoundingClientRect(); //获得在视口坐标中的位置
var offsets = getScrollOffsets();
var x = box.left + offset.x;
var y = box.top + offset.y
var w = box.width || (box.right - box.left);
var h = box.height || (box.bottom - box.top);

// 滚动条位置
function getScrollOffsets(w){
    w = w || window;

    // 除ie8- 版本，其他浏览器都能用
    if(w.pageXOffset != null) return {x: w.pageXOffset, y: w.pageYOffset};

    // 对标准模式下的浏览器
    var d = w.document;
    if(document.compatMode == "CSS1Compat"){
        return {x: d.documentElement.scrollLeft, y： d.documentElement.scrollTop};
    }

    // 对怪异模式下的浏览器
    return {x: d.body.scrollLeft, y: d.body.scrollTop};
}

```
----
## 十二、createDocumentFragment
- 作为其他节点的一个临时容器，是独立的，通常用在 创建文档片段，将元素附加到文档片段，然后将文档片段附加到DOM树
栗子：（from JavaScript权威指南）
```js
// 倒叙排列节点n的子节点
function reverse(n){
    // 创建一个DocumentFragment作为临时容器
    var f = document.createDocumentFragment();

    // 从后至前循环子节点，将每一个子节点移动到文档片段中
    // 注意：给f添加一个节点，改节点自动的会从n中删除
    while(n.lastChild) f.appendChild(n.lastChild);

    // 把f 的所有子节点一次性全部移回n中
    n.appendChild(f);
}
```
---

## 十三、defaultView / getComputedStyle()
### 1、defaultView
- 在浏览器中，该属性返回当前 document 对象所关联的 window 对象，如果没有，会返回 null
- 只读
- IE 9 以下版本不支持
- 使用方式： document.defaultView

### 2、getComputedStyle
- 是一个可以获取当前元素所有最终使用的CSS属性值，返回的是一个CSS样式声明对象([object CSSStyleDeclaration])，只读
- 语法：window.getComputedStyle(element, [pseudoElt]);
- element:用于获取计算样式的Element
- pseudoElt: 可选，指定一个要匹配的伪元素的字符串。必须对普通元素省略（或null）。

栗子1：（from MDN web docs）
```js
let elem1 = document.getElementById("elemId");
let style = window.getComputedStyle(elem1, null);

// 它等价于
// let style = document.defaultView.getComputedStyle(elem1, null);

```
栗子2：(from MDN web docs）
```html
<style>
 #elem-container{
   position: absolute;
   left:     100px;
   top:      200px;
   height:   100px;
 }
</style>

<div id="elem-container">dummy</div>
<div id="output"></div>

<script>
  function getTheStyle(){
    let elem = document.getElementById("elem-container");

    //getPropertyValue()返回一个DOMString含有一个指定的CSS属性的值
    let theCSSprop = window.getComputedStyle(elem,null).getPropertyValue("height");
    document.getElementById("output").innerHTML = theCSSprop;
   }
  getTheStyle();
</script>
```

待续。。。








