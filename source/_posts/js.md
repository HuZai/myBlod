---
title: js引擎执行过程
date: 2018-08-01
tags:
	- javascript
---
---
js是解释型的编程语言 单线程 但是通过EventLoop异步执行
---
### js 引擎执行过程分三个阶段
    1.语法分析
    2.预编译阶段 （变量提升在此阶段 变量提升是放到内存中 提升）
    3.执行阶段
#### 一.语法分析
##### 变量提升
    JavaScript引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行。这造成的结果，就是所有的变量的声明语句，都会被提升到代码的头部，这就叫做变量提升（hoisting）。