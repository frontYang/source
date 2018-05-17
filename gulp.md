---
title: gulp学习笔记
date: 2018-03-17 16:18:08
tags: [gulp]
categories: [学习]
---

## 一、安装
- `yarn global add gulp`
- 运行：`gulp tast名`
- 项目目录结构(示例)
 <!-- more -->
```
  + src(项目原目录)
    * images(图片)
      - icon(需要生成精灵图的图片)
        + icon1.png
        + icon2.png
      - pic.png
    * js(页面js)
      - block(公共js)
        + common.js
        + util.js
      - index.js
    * css(页面css)
      - block(公共css)
        + reset.scss
        + header.scss
      - index.scss
    * components(公共模块)
      - header.html
      - footer.html
    * index.html
  + dist(生成目录，不需创建)
    * images
      - sprite.png
      - pic.png
    * js
      - block
        + common.js
        + util.js
      - index.js
    * css
      - block
        + reset.css
        + header.scss
        + sprites.css
      - index.css
    * index.html
  + node_modules(node模块，不需创建)
  + gulpfile.js(gulp配置js)
  + package.json
  + yarn.lock
```
## 二、部分插件的安装和使用

### 1、browser-sync：自动刷新
- 安装： `yarn add browser-sync`
```js
const browserSync = require('browser-sync');

gulp.task('browserSync', function(){
    browserSync({
        server: {
            baseDir: 'dist'
        }
    })
})
```

### 2、处理css相关插件
- 安装： `yarn add gulp-sass gulp-postcss autoprefixer del-gulpsass-blank-lines`
```js
const sass = require('gulp-sass'); // 编译Sass
const postcss = require('gulp-postcss'); // 后处理器
const autoprefixer = require('autoprefixer'); // 加前缀，与postcss配合使用
const dgbl = require("del-gulpsass-blank-lines"); // 删掉sass空行

gulp.task('sass', function(){
  return gulp.src('src/css/**/*.scss')
  .pipe(sass({outputStyle: 'compact'}).on('error', sass.logError))
  .pipe(dgbl())
  .pipe(postcss([autoprefixer({browsers: ['last 2 versions', 'Android > 4.4','iOS >= 8', 'Firefox >= 20']})]))
  .pipe(gulp.dest('dist/css'))
  .pipe(browserSync.reload({  // 只有被改变的地方局部刷新
    stream: true
  }))
})
```

### 3、gulp-babel：es6转es5
- 安装： `yarn add gulp-babel`
```js
const babel = require('gulp-babel'); // bable

gulp.task('babel', function(){
  return gulp.src('src/js/**/*.js')
  .pipe(babel({
    presets: ['@babel/env'],
    plugins: ['@babel/transform-runtime']
  }))
  // .pipe(uglify())
  .pipe(gulp.dest('dist/js'))
})
```

### 4、处理图片
- 安装： `yarn add gulp-imagemin gulp.spritesmith gulp-cache`
```js
const imagemin = require('gulp-imagemin'); // 优化图片
const spritesmith = require('gulp.spritesmith'); // 图片精灵
const cache = require('gulp-cache'); // 缓存代理任务。，减少图片重复压缩

gulp.task('images', function() {
  return gulp.src('src/images/icon/*.png')
    // Caching images that ran through imagemin
    .pipe(cache(imagemin({
      interlaced: true,
    })))
    .pipe(spritesmith({
        imgName:'images/sprite.png', //合并后大图的名称
        cssName:'css/block/sprite.css',
        padding:2// 每个图片之间的间距，默认为0px
        // cssTemplate:(data)=>{ // 可以自定义输出格式
        // // data为对象，保存合成前小图和合成打大图的信息包括小图在大图之中的信息
        //    let arr = [],
        //         width = data.spritesheet.px.width,
        //         height = data.spritesheet.px.height,
        //         url =  data.spritesheet.image
        //     // console.log(data)
        //     data.sprites.forEach(function(sprite) {
        //         arr.push(
        //             '.icon-'+sprite.name+
        //             '{'+
        //                 'background: url('+url+') '+
        //                 'no-repeat '+
        //                 sprite.px.offset_x+' '+sprite.px.offset_y+';'+
        //                 'background-size: '+ width+' '+height+';'+
        //                 'width: '+sprite.px.width+';'+
        //                 'height: '+sprite.px.height+';'+
        //             '}\n'
        //         )
        //     })
        //     // return "@fs:108rem;\n"+arr.join("")
        //     return arr.join('')
        // }
    }))

    .pipe(gulp.dest('dist/'))
});
```

### 5、gulp-content-includer： include 公共模块
- 安装： `yarn add gulp-content-includer`
```js
const contentIncluder = require('gulp-content-includer');

gulp.task('concat',function() {
    gulp.src('src/*.html')
        .pipe(contentIncluder({ // 可以自定义
            includerReg:/<!\-\-\#include\s+virtual="([^"]+)"\-\->/g
        }))
        .pipe(gulp.dest('dist/'))
});

// 可以使用 <!--#include virtual="./components/header.html"--> 方式来引入公共模块
```

### 6、清理生成文件
- 安装： `yarn add del`
```js
const del = require('del'); // 清理生成文件

// 删除 除了images/文件夹，dist下的任意文件
gulp.task('clean:dist', function() {
  return del.sync(['dist/**/*', '!dist/images', '!dist/images/**/*']);
});

// 在某些时候我们还是需要清除图片，所以clean任务我们还需要保留
gulp.task('clean', function() {
  return del.sync('dist').then(function(cb) {
    return cache.clearAll(cb);
  });
})

```

### 7、组合任务
- 安装： `yarn add run-sequence`
```js
const runSequence = require('run-sequence'); // 按照指定顺序运行任务

// 监听文件，刷新浏览器
gulp.task('watch', function() {
  gulp.watch('src/scss/**/*.scss', ['sass']);
  gulp.watch('src/**/*.html', browserSync.reload);
  gulp.watch('src/js/**/*.js', browserSync.reload);
})

// 按顺序执行任务，放 [] 内的任务会同时执行
gulp.task('build', function(callback) {
  runSequence(
    'clean:dist',
    ['sass','babel','concat','images'],
    callback
  )
})

gulp.task('default', function(callback) {
  runSequence(['sass', 'babel', 'browserSync', 'concat', 'images'], 'watch',
    callback
  )
})

```



> [参考连接](https://www.cnblogs.com/Tom-yi/p/8036730.html)
> [仓库地址](https://github.com/frontYang/study/tree/master/gulp)
