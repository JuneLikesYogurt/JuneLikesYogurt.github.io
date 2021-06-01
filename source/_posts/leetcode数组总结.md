---
title: 【leetcode】数组部分总结
date: 2021-06-01 10:59:57
tags:
    - leetcode
    - Algorithm
categories:
    - Notes
---
参考[有没有人一起从零开始刷力扣](!https://leetcode-cn.com/circle/article/48kq9d/)的做题顺序，对于数组篇的总结
<!-- more -->
--- 
**数组是存放在连续内存空间上的相同类型数据的集合**

### key point
1. 数组下表都是从0开始的
2. 数组内存空间的地址是连续的
   
``` 
二维数组在内存中是n条连续的地址空间
```

---
## 常用方法
#### 双指针
#### 滑动窗口
根据当前子序列和大小的情况，不断调节子序列起始位置。从而将O(n^2)的暴力解法降为O(n)
#### 模拟行为
比如，顺时针/对角线输出数组

---
## 进一步参考链接
[没有人一起从零开始刷力扣(一)——数组篇](!https://leetcode-cn.com/circle/article/oalBEI/)
[LeetCode数组类知识点&题型总结](!https://blog.csdn.net/shulixu/article/details/90477714)
[Leetcode数组题型总结篇](!https://blog.csdn.net/Action_now_zj/article/details/116454158?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_title-0&spm=1001.2101.3001.4242)
