# 面试题收集
## 2018/10/09
- 清除浮动的方式和优缺点
  - clear清除浮动
    - 在浮动元素下方添加空div，并给该元素写css样式 `{clear: both; height: 0; overflow: hidden;}`
  - 给浮动元素的父级元素设置高度
  - 父级元素同时浮动（需要给父级同级元素添加浮动）
  - 父级设置成`inline-block`，其`margin:0 auto` 居中方式会失效
  - 给父级添加`overflow: hidden`
  - 万能清除法`after`伪类清浮动(推荐使用)
    ``` css
      .float_div::after{
        content: ".";
        clear: both;
        display: block;
        height: 0;
        overflow: hidden;
        visibility: hidden;
      }
      .float_div{
        zoom: 1;
      }
    ```
- px，em，rem的区别
  - px
    - 绝对长度单位（相对于显示器屏幕分辨率）
    - ie无法调整那些使用px作为单位的字体大小
    - 国外大部分网站能够调整的原因在于其使用了em或rem作为字体单位
    - firefox能够调整px和em、rem
  - em
    - 相对长度单位（相对于当前对象内文本的字体尺寸）
    - 值不固定
    - 会继承父级元素的字体大小
    - 为了简化font-size的换算，一般都会写入 `body{font-size: 62.5%; /* 16px * 62.5% = 10px */}`, 这样 `1em = 10px`;
  - rem：
    - css3新增
    - 相对单位（相对于html根元素）
    - 集相对大小和绝对大小的优点于一身，既可以做到只修改根元素就成比例地调整所有字体大小，又可以避免字体大小逐层复合的连锁反应
    - ie8及以下，Safari 4, IOS3.2不兼容


- css有哪些选择器，怎么计算优先级
  - `*` 通用选择器
  - id选择器 （权值 100）
  - class选择器，属性选择器，伪类选择器 （权值 10）
  - 标签选择器 （权值 1）
  - `>` 子选择器
  - `+` 毗邻选择器
  - 权值相加，权值较大的，优先级越高；权值相同的，后定义的优先级较高；样式含有 `!important`，优先级最高

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
  ```js
  function add(arr){
    return arr.slice(0, 50).reduce((a, b) => { return a + b })
  }
  ```

- 写一个你工作中用到的前端架构（如果有后端的也写上）

- 网页性能优化的方式（结合自己做的项目）

- vue 性能优化

- 微信小程序性能优化

- 最满意的项目碰到过的难点，怎么解决的



