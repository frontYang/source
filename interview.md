# 面试题收集
## 2018/10/09
- 清除浮动的方式和优缺点

- px，em，rem的区别

- 下面代码输出什么
```js
var a;

console.log(a(10, 20));

function a(x, y){
  return x > 0 ? (--x && a(x, y)) : (x * y);
}
```
答案：0

- 下面代码输出顺序
```js
var a = 1, b = 2, c = 3, d = 4;

console.log(a);

setTimeout(function(){
  console.log(b);
}, 100);

console.log(c);

setTimeout(function(){
  console.log(d);
}, 0);
```
答案：1,3,4,2

- 一个100位数的number数组，优雅的计算前50位之和

- 写一个你工作中用到的前端架构（如果有后端的也写上）

- 网页性能优化的方式（结合自己做的项目）

- vue 性能优化

- 微信小程序性能优化

- 最满意的项目碰到过的难点，怎么解决的



