---
title: BFC模型
date: 2020-03-11 14:45:56
tags:
    - CSS
    - 前端刷题
    - 面试
categories:
    - Notes
---
# 关于盒模型的一点唠叨
**W3C标准盒模型**，width和height等于内容区(content)的宽高；  
**IE盒模型**，width和height等于content+padding+border的总宽高。

## CSS3中对盒模型的类型设置属性
+ box-sizing: content-box; —— 标准盒模型
+ box-sizing: border-box; —— IE盒模型

## JS中对盒模型样式属性的一些操作
+ 读取元素的内联样式属性及修改元素的样式属性  

        元素.style.样式名 / 元素.style.样式名 = 样式值

+ 获取元素的当前样式对象
  
        元素.currentStyle / getComputedStyle(元素, null)

+ 读取元素的可见宽度和高度(content+padding) 
  
        元素.clientWidth / 元素.clientHeight

+ 读取元素的整个宽度和高度(content+padding+border)
  
        元素.offsetWidth / 元素.offsetHeight

# BFC
## CSS常见三种布局模型
+ 流动模型（Flow）
+ 浮动模型（Float）
+ 层模型（Layer）


## BFC概念
BFC 即 Block Formatting Contexts (块级格式化上下文)，它属于上述布局模式的**流动模型**。决定了元素如何对其内容进行定位，以及与其他元素的关系和相互作用。  
具有BFC特性的元素可以看做是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且BFC具有普通容器所没有的的一些特性。

## 形成BFC的条件
    以下任意一项满足即可
+ body根元素
+ 浮动元素： float除none以外的值
+ 绝对定位元素：position（absolute、fixed）
+ display 为inline-block、table-cells、flex
+ overflow 除了visible以外的值（hidden、auto、scroll）


## BFC作用
1. 阻止外边距折叠
   
    问题案例：**margin塌陷问题**：在标准文档流中，块级标签之间竖直方向的margin会以大的为准，这就是margin的塌陷现象。可以用overflow：hidden产生bfc来解决。
2. 包含浮动元素
    问题案列：**高度塌陷问题**，在通常情况下父元素的高度会被子元素撑开，而在这里因为其子元素为浮动元素所以父元素发生了高度坍塌，上下边界重，这时就可以用BFC来清除浮动了。
3. 阻止元素被浮动元素覆盖
    问题案例：**div浮动兄弟问题**：由于左侧块级元素发生了浮动，所以和右侧未发生浮动的块级元素不在同一层内，所以会发生div遮挡问题。可以给右侧元素添加 overflow: hidden，触发BFC来解决遮挡问题。


# 参考
[深入理解BFC原理及其在布局中的应用](https://blog.csdn.net/longyin0528/article/details/80787239)
[BFC原理和作用](https://www.jianshu.com/p/a9314f045898)