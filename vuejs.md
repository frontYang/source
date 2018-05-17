---
title: vue学习
date: 2017-04-04 12:52:57
tags: [vue]
categories: 学习
---

### 一、Vue.js组件的重要选项
#### 1、data: vue对象的数据
#### 2、methods: vue对象的方法
#### 3、watch: 设置了对象监听的方法
<!-- more -->

```html
<p>{{ a }}</p>

<script>
new Vue({
    data: {
        a: 1,
        b: []
    },
    methods: {
        doSometh: function(){
            console.log(this.a)
        }
    },
    watch: {
        'a': function(val, oldVal){
                console.log(val, oldVal)
        }
    }
})
</script>
```
---
### 二、模板指令——html和vue对象的粘合剂

#### 1、数据渲染：
```
v-text、v-html、 {{ }}
```

```html
<p>{{ a }}</p>
<p v-text="a"></p>
<p v-html="a"></p>
<script>
new Vue({
    data: [
        a: 1,
        b: []
    ]
})
<script>
```

#### 2、控制模块隐藏： v-if、v-show

```html
<p v-if="isShow"></p>
<p v-show="isShow"></p>

<script>
    new Vue({
    data: {
        isShow: true
    }
})
</script>

```

#### 3、渲染循环列表： v-for
```html
<ul>
    <li v-for='item in items'>
        <p v-text='item.label'></p>
    </li>
</ul>

<script>
...
data: {
    items: [
        {
            label: 'apple'
        },
        {
            label: 'banana'
        }
    ]
}
...
</script>
```

#### 4、事件绑定： v-on
```html
<button v-on:click="doThis"></button>
<button @click="doThis"></button>
<script>
...
nethods: {
    doThis: function(someThing){}
}
...
</script>

```

#### 5、属性绑定： v-bind
```html
<img v-bind:src="" alt="">  //字符串
<img :src="" alt="">

<div ：class="{ red: isRed }"></div> //布尔值
<div ：class="[classA, classB]"></div> //
<div ：class="[classA, {classB: isB, classC： isC}]"></div>

```
未完待续。。。

