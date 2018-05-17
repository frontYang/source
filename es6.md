---
title: ES6学习笔记
date: 2017-08-09 20:20:33
tags: [ES6, javascript]
categories: 学习
---

## 一、var、let、const

### 1、var: 无论在代码的那个地方声明，都会提升到该函数的开头（变量提升）；
栗子：
```js
for(var i = 0; i < 10; i ++){

}

console.log(i); //10
```
<!-- more -->
### 2、let：用法类似于var，但不会变量提升，只在let命令所在的代码块内有效；
栗子：
```js
for(let i = 0; i < 10; i ++){

}

console.log(i); //i is not defined

```

### 3、const: 声明常量，一旦声明，不可更改（不是变量的值不能改动，而是变量指向的那个内存地址不得改动，见例3），而且常量必须初始化赋值，和let一样，只在声明所在的块级作用域内有效；
栗子1：
```js
// 更改值的情况
const PI = 3.1415;
console.log(PI);
PI = 3;
// Assignment to constant variable.
```
栗子2：
```js
// 未赋值的情况
const PI
// Missing initializer in const declaration

```
栗子3：
```js
// 对象声明
const foo = {};
foo.prop = 123;
console.log(foo.prop) // 123

foo = {} //Assignment to constant variable.
```
---

## 二、解构赋值
### 1、数组的解构赋值
- 从数组中提取值，按照对应位置，对变量赋值
```js
let [a, b, c] = [1, 2, 3];
```

待续。。。

<!-- ##### 对象的解构赋值
##### 字符串的解构赋值
##### 数值和布尔值的解构赋值
##### 函数参数的解构赋值
 -->


