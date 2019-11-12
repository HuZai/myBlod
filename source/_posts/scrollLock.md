---
title: safari弹层局部滚动触发全局滚动bug 处理
date: 2019-11-12
tags:
	- javascript
---
---
safrai里 弹框 使用局部滚动条 导致底下全局滚动条被触发bug 兼容处理方案
---
###使用  [body-scroll-lock](https://github.com/willmcpo/body-scroll-lock)
    1.要滚动的区域加 popup可滑动 其他区域不可滑动
        const targetElement = document.getElementById("popup"); //only popup can scroll
        //put this when popup opens, to stop body scrolling
        bodyScrollLock.disableBodyScroll(targetElement);
    2 取消锁定
        //put this when close popup and show scrollbar in body
        bodyScrollLock.enableBodyScroll(targetElement);