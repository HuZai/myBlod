---
title: es6 容易遗漏点
date: 2018-08-01
tags:
	- es6
---
### import 执行时机
    ES6 import是静态编译 也就是预编译时加载 
    javascript是边编译边执行
    import 在预编译阶段 做一份对属性和方法的指针引用
    所以不能执行 import {'a'+'b'}
 与 CommonJS 区别

    CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
    CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。
    
    import  对引入对象修改回 会对原对象产生修改
    require 是复制一个新对象 不会对原对象产生修改
    
    require 是运行时 执行所以可以执行 require()