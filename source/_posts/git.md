---
title: git
date: 2018-08-01
tags:
	- git
---
### fork 项目同步
```$xslt
//查看当前分支
$ git remote -v
origin	http://XXXX.git (fetch)
origin	http://XXXX.git (push)
//加一个「引用 」到上游项目，别称为 upstream。
$ git remote add upstream https://github.com/xxx.git
//查看上游项目，别称为 upstream
$ git remote -v
origin	http://XXXX.git (fetch)
origin	http://XXXX.git (push)
upstream	https://github.com/xxx.git (fetch)
upstream	https://github.com/xxx.git (push)

//更新 同步
$ git fetch upstream
```
### git 同步远程分支列表
```$xslt
// 产看所有分支列表
$ git branch -a
//拉取更新远程分支列表
$ git remote update origin --prune
// 结果如下
Fetching origin
From http://XXXX
 - [deleted]           (none)     -> origin/UP1953
 - [deleted]           (none)     -> origin/edit
 - [deleted]           (none)     -> origin/up1827
```
### check 远程分支到本地 另命名
 ```$xslt
git checkout -b 本地分支名x origin/远程分支名x
```
### git 添加远程分
 ```$xslt
 git remote add origin XXXXX
 git push origin master
 ```
