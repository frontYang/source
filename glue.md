---
title: Glue安装步骤及常用命令
tags: [glue]
categories: 学习
---

## 一、windows安装步骤
- 安装[Python](http://www.python.org/ftp/python/2.7.2/python-2.7.2.msi)
- 安装[PIL](http://www.lfd.uci.edu/~gohlke/pythonlibs/xn3pw759/PIL-1.1.7.win32-py2.7.exe)
- 安装 Python 的 [easy_install](http://pypi.python.org/packages/2.7/s/setuptools/setuptools-0.6c11.win32-py2.7.exe)
- 添加Python的脚本目录到path
- 运行如下命令 **easy_install glue**
<!--more-->

## 二、常用命令
- 1.快速: glue 源文件夹名称 输出文件夹名称
`glue source output`

- 2.排序：
`glue source output --square|vertical|hortizontal|diagonal|vertical-right|horizontal-bottom`

- 3.去掉图片多余的空白
`glue source output --crop`

- 4.在不同的文件夹下生成css和img
`glue source --img=images/compiled --css=css/compiled`

- 5.生成一个测试的html
`glue source output --html`

- 6.更改文件格式
`glue source output --less`

- 7.展开间距并且不计算为宽高
`glue source output --10`
`glue source output --'10 20'`
`glue source output --'10 20 30 40'`

- 8.排序
`glue source output --maxside|width|height|area|filename`
`glue source output ---maxside|-width|-height|-area|-filename`

- 9.在图像周围填充间距
`glue source output --10`
`glue source output --10 20`
`glue source output --10 20 30 40`

- 10.图片格式
`glue source output --png8`

- 11.后台监听
`glue source output --watch`

[参考](http://glue.readthedocs.io/en/latest/options.html)
