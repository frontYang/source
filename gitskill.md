---
title: Git常用命令
date: 2018-03-11 15:19:58
tags: [git]
categories: 学习
---

## 创建版本库
- 初始化一个Git仓库，使用 `git init` 命令
- 添加文件到仓库:
  - 第一步：使用命令 `git add [file]`，可以反复多次使用，添加多个文件
  - 第二步：使用命令 `git commit`
---
<!-- more -->

## 查看仓库目前状态
- `git status`:掌握工作区的状态,查看文件是否被修改过(但还没有准备提交的修改)
- `git diff`:可以查看修改内容
    + `git diff` 比对当前内容和暂存区内容。
    + `git diff HEAD` 比对当前内容和最近一次提交。
    + `git diff HEAD^` 比对当前内容和倒数第二次提交。
    + `git diff HEAD^ HEAD` 比对最近两次提交。
---

## 版本回退
- HEAD指向的版本就是当前版本，git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`
- `git log`查看提交历史，以便会退到哪个版本
    + `git log [--oneline] [--all]` 查看提交历史。
    + `git log --oneline` 打印为单行log。
    + `git log --all`  打印所有记录（忽略HEAD的位置）。
    + `git log --graph` 打印示意图（忽略HEAD的位置）。

- `git reflog`查看命令历史，以便确定要回到未来哪个版本
---

## 撤销修改
- `git checkout -- file`:直接丢弃工作区的修改
- 丢弃暂存区的修改:用`git reset HEAD file`回到上一步
- 已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库
---

## 删除文件
- 第一步：`git rm [file]`
- 第二步：`git commit`
---

## 添加远程库
- 要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`
- 关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容
- 此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改
- ps:由于公司防火墙通过ssh不能连接github，需要设置http代理来连接
  - 设置代理：`git config --global http.proxy 192.168.xx.xxx:xxxx`
  - 移除代理：`git config --system --unset http.proxy`
---

## 从远程库克隆
- `git clone https://github.com/xx/xx.git`
- 拉取远程仓库 `git pull`
---

## 分支管理
- 查看分支:`git branch`
- 创建分支：`git branch [name]`
- 切换分支：`git checkout [name]`
- 创建+切换分支：`git checkout -b [name]`
- 合并某分支到当前分支：`git merge [name]`
- 删除分支：`git branch -d [name]`
---

## 修改文件后提交 并提交到github 步骤：
- 创建并切换分支: `git checkout -b [name]`
- 添加修改过的文件: `git add [file]`
- 提交文件: `git commit -m "描述"`
- 切换到master分支: `git checkout [name]`
- 合并修改文件的分支到当前分支: `git merge [name]`
- 删除修改文件的分支: `git branch -d [name]`
- 提交到远程库: `git push origin master`

[参考](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)


