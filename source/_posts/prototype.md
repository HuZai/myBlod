---
title: js 原型 原型链 构造函数
date: 2019-07-05
tags:
	- js
	- es6
---
##### 普及知识点
    1. __proto__ 即在ES标准定义中的[[Prototype]] 两者是同一个东西 也可称为 隐式原型
    2. __proto__  属性（也叫隐式原型属性） 是对象拥有 是一个指针 指向 原型对象
    3. prototype属性 是函数独有 但是 函数也是特殊的对象所以 函数也拥有__proto__  
    4. 一切皆函数 Object对象本身也是函数
    作用
       1.__proto__ 指向父对象的 prototype 形成原型链表 当前对象查找自身属性找不到是会通过它 去找父对象原型上查找
       层层往上  最上层是 null 即 Object.prototype.__proto__ == null
       2.prototype 作用 可以让多个子类公同拥有的属性或者方法 即实例共享的属性和方法
         prototype 指向原型对象 
#### 场景
    function People(){ // 类
        this.name='人类'
    } 
    People.prototype.say=function(){
        console.log('my name is '+this.name)
    }
    let man =new People() //实例 
### 原型对象 以及prototype 属性
    1.当在声明一个函数(People)时 浏览器就会按照一定规则创建一个对象 这个对象就是原型对象（People.prototype），存在内存里。
    2.声明的这个函数(People)会有一个prototype属性它是一个指针，指向原型对象。
    3.这个对象的用途就是包含所有实例共享的属性和方法;
    4.原型对象（People.prototype ）含有一个constructor属性 指向构造函数（People自身）[这条可以暂时忽略往下看 constructor属性]
例：场景 声明PeoPle() 浏览器 就会生成一个原型对象（People.prototype） 并且People() 会有要一个prototype 属性指向此原型对象    
**原型对象 与原型属性关系如下图。 原型属性指向原型对象** 


![](/img/class/prototype_only.png)
### __proto__ 隐式原型
    __proto__ 指向父类的 prototype
    man.__proto__ == People.prototype
**如下图所示** 
![](/img/class/constructorExtends.png)
### constructor属性
```$xslt
    1.constructor 和 __proto__是对象才拥有 因为函数也是对象 所以函数也拥有
    2.constructor 属性是【原型对象】上的一个属性 
    3.constructor属性 指向 对象的构造函数
```
    例：
    1.People.prototype 原型对象 的constructor 属性指向自身People 构造函数
    2.man自身是没有 constructor 的 是通过继承而来 
      man.__proto__ == People.prototype
      即 man.__proto__.constructor 
**如下图所示**
![](/img/class/constructorExtends.png)
**People的构造函数如下图所示**

    People.__proto__ == Function.prototype
    People.constructor == Function.prototype.constructor
    
![](/img/class/constructor1.png)
### 原型链
原型链 通过 __proto__ 层层 网上找 最终找到 Object.prototype.__proto__ 指向null
**如下图所示**
![](/img/class/prototypes.png)

[参考链接: https://blog.csdn.net/cc18868876837/article/details/81211729](https://blog.csdn.net/cc18868876837/article/details/81211729) 

相关 继承 等面试题看 '{% post_link classEs5 %}' 这篇文章
