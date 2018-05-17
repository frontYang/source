---
title: 前端面试知识点收集
date: 2018-04-01 15:14:57
tags: [面试]
categories: 面试
---

## HTML篇
### doctype
> doctype 是用来告知浏览器的解析器用什么文档标准解析这个文档

### HTML5 为什么只需要写 `<!DOCTYPE HTML>`
> HTML5 不基于 SGML，因此不需要对DTD进行引用，但是需要doctype来规范浏览器的行为（让浏览器按照它们应该的方式来运行）；
> 而HTML4.01基于SGML,所以需要对DTD进行引用，才能告知浏览器文档所使用的文档类型。

<!-- more -->

### link/@import区别
- link
  - 属于XHTML标签，除了加载CSS外，还能用于定义RSS, 定义rel连接属性等作用
  - 页面被加载的时，link会同时被加载
  - 支持使用js控制DOM去改变样式
- @import
  - 是CSS提供的，只能用于加载CSS
  - 页面被加载的时，等到页面被加载完再加载
  - 不支持使用js控制DOM去改变样式

<!-- ### 浏览器内核
### html5 -->
### 语义化
- html语义化让页面的内容结构化，结构更清晰，便于对浏览器、搜索引擎解析
<!--  ### viewport -->

---

<!-- more -->

## Javascript篇
<!-- ### dom -->
### 原型/原型链
<img src="/images/basic/prototype.png" width="90%">

<!-- ### 作用域/动态作用域（实际面试中遇到过）
### js执行机制
### 闭包
### this/call/apply/bind
### 跨域
### 工具函数(节流/防抖/扁平/柯里化...)
### 模块化
### 常用设计模式 -->
### cookies/sessionStorage/localStorage的区别
- cookie
    - cookie是网站为了标示用户身份而存储在用户本地终端上的数据，通常经过加密
    - cookie数据始终在同源的http请求中携带（即使不需要），即会在浏览器和服务器之间来回传递
    - cookie数据大小不能超过4k
    - 设置cookie的过期时间之前一直有效，即使窗口或浏览器关闭
- sessionStorage和localStorage
    - 不会自动把数据发给服务器，仅在本地保存
    - 存储大小比cookie大得多，可以达到5M或更大
- sessionStorage
  + 数据在当前浏览器窗口关闭后自动删除
- localStorage
  + 存储持久数据，浏览器关闭后数据不丢失除非主动删除数据

<!-- ### 正则
### es6
### promise
### vue
### ajax
### 堆和栈
### 定时器 -->

### 数组去重
- 双层循环
```js
function unique(array){

    // 存储结果
    var res = [];

    for(var i = 0, arrLen = array.length; i < arrLen; i++){
        for(var j = 0, resLen = res.length; j < resLen; j++){
            if(array[i] == res[j]){
                break;
            }
        }

        // 如果array[i]是唯一的，那么执行完循环，j等于resLen
        if(j === resLen){
            res.push(array[i])
        }
    }

    return res;
}
```

- indexOf
```js
function unique(array){
  var res = [];

  for(var i = 0, len = arr.length; i < len; i++){
    var current = array[i];

    if(res.indexOf(current) === -1){
      res.push(current);
    }
  }

  return res;
}
```
- 排序后去重
```js
function unique(array){
  var res = [];
  var sortedArray = array.concat().sort();
  var seen;

  for(var i = 0, len = sortedArray.length; i < len; i++){

    // 如果是第一个元素或者相邻的元素不相同
    if(!i || seen !== sortedArray[i]){
      res.push(sortedArray[i]);
    }

    seen = sortedArray[i];
  }

  return res;
}
```

- 深入
```js
function unique(array, isSorted){
  var res = [];
  var seen = [];

  for(var i = 0, len = array.length; i < len; i++){
    var value = array[i];

    if(isSorted){
      if(!i || seen !== value){
        res.push(value)
      }
      seen = value;
    }else if(res.indexOf(value) === -1){
      res.push(value);
    }
    return res
  }
}
```
- 优化
```js
// 迭代
function unique(array, isSorted, iteratee){
  var res = [];
  var seen = [];

  for(var i = 0, len = array.length; i < len; i++){
    var value = array[i];
    var computed = iteratee ? iteratee(value, i, array) : value;

    if(isSorted){
      if(!i || seen !== value){
        res.push(value);
      }

      seen = value;
    }else if(iteratee){
      if(seen.indexOf(computed) === -1){
        seen.push(computed);
        res.push(value);
      }
    }else if(res.indexOf(value) === -1){
      res.push(value);
    }
  }

  return res;
}

```

- fiter(ES5)
```js
function unique(array){
  var res = array.filter(function(item, index. array){
    return array.indexOf(item) === index;
  })

  return res;
}
```

<!-- ---

## css篇
### css布局
### BFC(Block Formatting Context, 格式化上下文)
### flex
### 移动端
### css3
### css3动画优化

---

## 工具篇
### webpack
### gulp
### 版本控制问题(git/svn)

---


## 其他
### 前端规范
### PC兼容性
### 移动端兼容性
### 调试技巧
### 常见前端数据结构与算法
### 前端部署代码问题
### 浏览器问题
### 前端安全 -->

待续。。。

