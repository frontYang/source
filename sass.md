---
title: Sass学习笔记
date: 2017-03-27 15:58:57
tags: [sass]
categories: 学习
---

## 一、常用命令
- 项目中常用：
`sass --watch sass:css --style compact`

- 格式转换
`sass-convert style.sass style.scss`

- 运行sass
`sass input.css output.css`
<!--more-->
- 监视sass文件，每次更新时，自动编译成css
`sass --watch input.scss: output.css`

- 监视整个文件夹
`sass --watch app/sass:public/stylesheets`

- 更多命令行
`sass --help`

- 紧凑输出方式 campact
`sass --watch test.scss:test.css --style compact`

- 压缩输出方式 campact
`sass --watch test.scss:test.css --style compressed`

---
## 二、基本用法
### 1、变量
- 使用$开头
```css
$blue : #1875e7;
div{
    color: $blue;
}
```

- 如果变量需要镶嵌在字符串中，就必须需要写在#{}之中
```
$side: left;
.rounded {
    border-#{$side}-radius: 5px;
}
```

### 2、计算功能
- 例如
```css
$var: 500px;
body{
    margin: (14px/2);
    top:50px + 100px;
    right:$var * 10%
}
```

### 3、嵌套
- 标签嵌套
```css
div{
    h1{
        color:red;
    }
}

```

- 属性嵌套
```css
p{
    border:{
        color:red;
    }
}
```

- 伪类
```css
a {
    &:hover{
        color:#ffb3ff;
    }
}
```

### 4、注释
- 会保留到编译后的文件
`/*  */`
- 只保留在sass源文件中，编译后被省略
`//`
- 主要注释，即使是压缩模式编译，也会保留这行注释，通常用作声明版权信息
```
/*!
    重要注释
*/
```
---

## 三、代码重用

### 4、继承
* 例如
```css
.class1{
    border: 1px solid #ddd;
}

.class2{
    @extend .class1;
    font-size: 120%;
}
```

### 5、Mixin
- 可以重用的代码块
```css
@mixin left($value: 10px){
    float: left;
    margin-right: $value;
}

div{
    @include left;
}
```

- 可以指定参数和缺省值
```css
@mixin rounded($vert, $horz, $radius: 10px){
    border-#{$vert}-#{$horz}-radius: $radius;
    -moz-border-#{$vert}-#{$horz}-radius: $radius;
    -webkit-border-#{$vert}-#{$horz}-radius: $radius;
}

#navbar li{
    @include rounded(top, left);
}

#footer{
    @include rounded(top, left, 5px);
}
```

### 6、颜色函数
```
lighten(#cc3, 10%) // #d6d65c
darken(#cc3, 10%) // #a3a329
grayscale(#cc3) // #808080
complement(#cc3) // #33c
```

### 7、插入文件
- @import命令，用来插入外部文件。
```css
　　@import "path/filename.scss";
```

- 如果插入的是.css文件，则等同于css的import命令。
```css
　　@import "foo.css"; 　　
```
---
## 四、高级用法
### 1、条件语句
* 例1
```css
p {
    @if 1 + 1 == 2 { border:1px solid #eee; }

    @if 5 < 3 { border:2px dotted #eee; }
}

等同于：

p {  border: 1px solid #eee; }

```

* 例2
```css
p{
    @if lightness($color) > 30% {
        background-color: #000;
    } @else {
        background-color: #fff;
    }
}

等同于：
p {  background-color: #000; }
```

### 2、循环语句
- for循环
```css
@for $i from 1 to 10{
    .border-#{$i}{
        border: #{$i}px solid blue;
    }
}

等同于：
.border-1 {border: 1px solid blue; }
.border-2 {border: 2px solid blue; }
.border-3 {border: 3px solid blue; }
.border-4 {border: 4px solid blue; }
.border-5 {border: 5px solid blue; }
.border-6 {border: 6px solid blue; }
.border-7 {border: 7px solid blue; }
.border-8 {border: 8px solid blue; }
.border-9 {border: 9px solid blue; }

```

- while循环
```css
$i: 6;
@while $i > 0{
    .item-#{$i}{
        width: 2em * $i;
    }

    $i: $i - 2;
}

等同于：
.item-6 {width: 12em; }
.item-4 {width: 8em; }
.item-2 {width: 4em; }
```
- each
```css
@each $member in a,b,c,d{
    .#{$member} {
        background-image:url("/image/#{$member}.jpg");
    }
}

等同于：
.a {background-image: url("/image/a.jpg"); }
.b {background-image: url("/image/b.jpg"); }
.c {background-image: url("/image/c.jpg"); }
.d {background-image: url("/image/d.jpg"); }

```

### 3、自定义函数
* 例如：
```css
@function double($n){
    @return $n * 2;
}

#sidebar {
    width: double(5px);
}

等同于：
#sidebar { width: 10px; }

```



> ps: [参考](http://www.ruanyifeng.com/blog/2012/06/sass.html),[安装](https://www.sass.hk/install/),[调试](http://www.w3cplus.com/preprocessor/sass-debug-with-developer-tool.html),[编译](http://www.w3cplus.com/preprocessor/sass-compile.html)

