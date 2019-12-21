---
title: safari弹层局部滚动触发全局滚动bug 处理
date: 2019-11-12
tags:
	- javascript
---
---
safrai里 弹框 使用局部滚动条 导致底下全局滚动条被触发bug 兼容处理方案
---
### 使用  [body-scroll-lock](https://github.com/willmcpo/body-scroll-lock)
    1.要滚动的区域加 popup可滑动 其他区域不可滑动
        const targetElement = document.getElementById("popup"); //only popup can scroll
        //put this when popup opens, to stop body scrolling
        bodyScrollLock.disableBodyScroll(targetElement);
    2 取消锁定
        //put this when close popup and show scrollbar in body
        bodyScrollLock.enableBodyScroll(targetElement);
### 源码里 对滚动事件 做的优化分析 passive 的处理
##### 1.  使用 passive 改善的滚屏性能
##### passive  出现原由

根据规范，passive 选项的默认值始终为false。但是，这引入了处理某些触摸事件（以及其他）的事件监听器在尝试处理滚动时阻止浏览器的主线程的可能性，从而导致滚动处理期间性能可能大大降低。

***为防止出现此问题，某些浏览器（特别是Chrome和Firefox）已将touchstart和touchmove事件的passive选项的默认值更改为true文档级节点 Window，Document和Document.body。这可以防止调用事件监听器，因此在用户滚动时无法阻止页面呈现。***


如果我们在 touchstart 事件调用 preventDefault 会怎样呢？这时页面会禁止，不会滚动或缩放。那么问题来了：浏览器无法预先知道一个监听器会不会调用 preventDefault()，它需要等监听器执行完后，再去执行默认行为，而监听器执行是要耗时的，这样就会导致页面卡顿。

通俗讲

当你触摸滑动页面时，页面应该跟随手指一起滚动。而此时你绑定了一个 touchstart 事件，你的事件大概执行 200 毫秒。这时浏览器就犯迷糊了：如果你在事件绑定函数中调用了 preventDefault，那么页面就不应该滚动，如果你没有调用 preventDefault，页面就需要滚动。但是你到底调用了还是没有调用，浏览器不知道。只能先执行你的函数，等 200 毫秒后，绑定事件执行完了，浏览器才知道，“哦，原来你没有阻止默认行为，好的，我马上滚”。此时，页面开始滚。

passive 不是所有浏览器都支持
##### 写出兼容性好的 设置
    var passiveSupported = false;
    
    try {
      var options = Object.defineProperty({}, "passive", {
        get: function() {
          passiveSupported = true;
        }
      });
    
      window.addEventListener("test", null, options);
    } catch(err) {}
    
    //监听touchmove时 开启
    document.removeEventListener('touchmove', preventDefault, passiveSupported ? { passive: true } : passiveSupported);
    
对上述代码解析
   window.addEventListener('testPassive' ...) 如果浏览器支持 passive 就回去读取 passive的getter passiveSupported 就会变成true
    
   那些不支持参数options的浏览器，会把第三个参数默认为useCapture，即设置useCapture为true 


[参考地址 页面下面](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener)

注意上述斜体部分： 部分浏览器 默认开启了passive true passive: true 时 stopPropagation 是不起作用的

因此body-scroll-lock里首先把passive: false  然后在达到页面滑动到最上或者最下面条件是用 stopPropagation()阻止事件传播 阻止触发底层全局滚动
