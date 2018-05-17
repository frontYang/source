---
title: 常用css收集
date: 2018-03-11 15:46:45
tags: [css]
categories: 学习
---

## 一、 reset
<!-- more -->

```css
@charset 'utf-8';
@charset "GBK";

*{margin:0;padding:0; -moz-box-sizing: border-box; -webkit-box-sizing: border-box; box-sizing: border-box;-webkit-tap-highlight-color:transparent;-webkit-touch-callout:none;-webkit-text-size-adjust:none; }
*:focus,*:active {outline:none;}
html, body {  height:100%;}
body{ width: 100%;font-family: \5FAE\8F6F\96C5\9ED1,\5B8B\4F53;-webkit-user-select:none;}
p,a,span,textarea,b,input,dt,dd { color: #666;font-size: 0.9rem;}
ul, ol{list-style:none;}
img{border:none;}
a { text-decoration:none;}
textarea {resize:none;}
input[type=button],button{text-align: center; background: none; border: 0; outline: none; }
input { background: white; border: none; outline: none;}

/*占位符颜色*/
input::-webkit-input-placeholder, textarea::-webkit-input-placeholder { color:#ccc; }
input:-moz-placeholder, textarea:-moz-placeholder { color:#ccc; }
input::-moz-placeholder, textarea::-moz-placeholder { color: #ccc; }
input:-ms-input-placeholder, textarea:-ms-input-placeholder { color: #ccc; }

/*占位符点击消失*/
input:focus::-webkit-input-placeholder{text-indent: -999em; z-index: -20; }
```
---

## 二、function
```css
/* 浮动 */
.fl { float: left !important; }
.fr { float: right !important; }
.clear { clear: both; }

/* 功能 */
.hide { display:none !important; }/*隐藏*/
.ellipsis1 { overflow:hidden;-o-text-overflow:ellipsis;text-overflow:ellipsis;white-space:nowrap;word-break:keep-all; } /* 字数省略 */
.ellipsis2 {display: -webkit-box; -webkit-line-clamp:2; overflow: hidden; -webkit-box-orient: vertical; text-overflow: ellipsis;} /* 两行字数省略 */
.keep { position:fixed !important;border-bottom: #ccc 1px solid !important; } /*滚动保持*/
.mask {display: none;position: fixed;left: 0;z-index: 11;width: 100%; height: 100%;background-color:rgba(0, 0, 0, 0.4);}/*遮罩层*/
```
---

## 三、skin
```css
/* 效果 */
.shadow,.all-shadow * {box-shadow: 0 0 .3rem #ddd !important;}
.gradient,.all-gradient * {background: -webkit-linear-gradient(top, #fff4f4,#fff);}
.radius,.all-radius * { border-radius: .2em;}
.left-radius {border-top-left-radius: .2em;border-bottom-left-radius: .2em;}
.right-radius {border-top-right-radius: .2em;border-bottom-right-radius: .2em;}
.top-radius {border-top-left-radius: .2em;border-top-right-radius: .2em;}
.alpha{filter:alpha(opacity=50); -moz-opacity:0.5; -khtml-opacity: 0.5; opacity: 0.5;}
```
---

## 四、arrow
```css
/* 纯css实现箭头样式 u:上 r:右 d:下 l:左; S*/
/**
 * 实心箭头
 */
.icon-arrA{display: inline-block;/* ie7下需要 加上inline-block 才生效,ie6不支持*/ font-size: 0; line-height: 0; overflow: hidden; }
.icon-arrA-u{border-width: 8px; border-style: solid; border-color: transparent transparent #e6e6e6 transparent; }
.icon-arrA-d{border-width: 8px; border-style: solid; border-color: #e6e6e6 transparent transparent transparent; }
.icon-arrA-l{border-width: 8px; border-style: solid; border-color: transparent #e6e6e6 transparent transparent; }
.icon-arrA-r{border-width: 8px; border-style: solid; border-color: transparent transparent transparent #e6e6e6; }

/**
 * 边框箭头 兼容webkit
 */
.icon-arrB{display: inline-block; width: 8px; height: 8px; font-size: 0; overflow: hidden; background-color: transparent; border-width: 2px 2px 0 0; border-style: solid; border-color: #e6e6e6; }
.icon-arrB-u{-webkit-transform: rotate(-45deg); -moz-transform: rotate(-45deg); -ms-transform: rotate(-45deg); transform: rotate(-45deg); }
.icon-arrB-d{-webkit-transform: rotate(135deg); -moz-transform: rotate(135deg); -ms-transform: rotate(135deg); transform: rotate(135deg); }
.icon-arrB-l{-webkit-transform: rotate(45deg); -moz-transform: rotate(45deg); -ms-transform: rotate(45deg); transform: rotate(45deg); }
.icon-arrB-r{-webkit-transform: rotate(-135deg); -moz-transform: rotate(-135deg); -ms-transform: rotate(-135deg); transform: rotate(45deg); }

/**
 * 边框箭头：兼容 ie（不包括ie6）
 */
.arr-wrap{display: inline-block; position: relative; }
.arr-wrap .icon-arrA{position: absolute; left: 0; }
.arr-wrap .s1.icon-arrA-u, .arr-wrap .s1.icon-arrA-r, .arr-wrap .s1.icon-arrA-d, .arr-wrap .s1.icon-arrA-l{z-index: 1; }
.arr-wrap .s1.icon-arrA-u, .arr-wrap .s2.icon-arrA-d{top: 2px; }
.arr-wrap .s2.icon-arrA-u, .arr-wrap .s1.icon-arrA-d{top: 0; }
.arr-wrap .s2.icon-arrA-u, .arr-wrap .s2.icon-arrA-r, .arr-wrap .s2.icon-arrA-d, .arr-wrap .s2.icon-arrA-l{z-index: 0; }
.arr-wrap .s1.icon-arrA-r, .arr-wrap .s2.icon-arrA-l{left: 0; }
.arr-wrap .s2.icon-arrA-r, .arr-wrap .s1.icon-arrA-l{left: 2px; }
.arr-wrap .s1.icon-arrA-u{border-bottom-color: #fff; }
.arr-wrap .s2.icon-arrA-u{border-bottom-color: #e6e6e6; }
.arr-wrap .s1.icon-arrA-r{border-left-color: #fff; }
.arr-wrap .s2.icon-arrA-r{border-left-color: #e6e6e6; }
.arr-wrap .s1.icon-arrA-d{border-top-color: #fff; }
.arr-wrap .s2.icon-arrA-d{border-top-color: #ddd; }
.arr-wrap .s1.icon-arrA-l{border-right-color: #fff; }
.arr-wrap .s2.icon-arrA-l{border-right-color: #e6e6e6; }

/* 箭头边框阴影 */
.arr-wrap-su{width: 20px;height: 20px;}
.arr-wrap-su::after{content: ''; position: absolute; z-index: -1; width: 8px; height: 8px; background: #fff; left: 4px; top: 12px; border-width: 1px 1px 0 0; border-style: solid; border-color: #e6e6e6; -webkit-transform: rotate(-45deg); -moz-transform: rotate(-45deg); transform: rotate(-45deg); box-shadow: 2px -2px 5px #e6e6e6; -webkit-box-shadow: 2px -2px 5px #e6e6e6; -moz-box-shadow: 2px -2px 5px #e6e6e6; }
/* 纯css实现箭头样式 E*/

```
---
## 五、ie6兼容问题
- 链接伪类（:hover）CSS背景图片有闪动
```html
<!--[if IE 6]><script>document.execCommand("BackgroundImageCache", false, true)</script><![endif]-->
```
- border兼容transparent
```css
 _border-color:tomato;_filter:chroma(color=tomato);
```
- 兼容fixed
```css
.fixed{position:fixed; top:35%; right:0; _position:absolute; _top:expression(offsetParent.scrollTop+100); _right:expression(offsetParent.scrollRight);}
```