---
title: git 创建空白分支
date: 2017-02-28 17:12:18
tags: [git]
categories: [git]
---

# git 创建空白分支

今天在部署hexo的博客的时候，需要在github上创建两个分支。一个是发布分支 master 一个是开发分支dev。在我把发布内容已经提交到master分支时，我忽然想起，需要另开一个分支来把部署的代码放上去。这时我需要再开一条空白分支。

# 代码

#### 创建一个分支

####  `git checkout --orphan dev`

#### 清空文件

`git rm -rf .`

#### 再按正常流程提交

`git commit -m "message"`

#### 在远程创建dev分支

`git push origin dev`

参考：[在git中创建一个空白分支](https://segmentfault.com/a/1190000004931751)





