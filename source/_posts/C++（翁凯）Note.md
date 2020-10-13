---
title: C++(翁凯）Note
date: 2019-03-26 14:51:35
tags:
    - C++
categories: 
    - Notes
---
翁凯老师C++课的笔记，强烈安利！（网易云课堂）
<!-- more -->

### 头文件
1. only **declarations** are allowed to be in .h(.h里放声明，不放定义)
   1. extern variables
   2. functions prototypes
   3. class/struct declaration
2. **#include**
   1. #include'xx': first search in the current directory,then the directories declared somewhere
   2. #include<xx.h>:   search int the specified directories(系统头文件目录,linux下/usr/include/)
   3. #include<xx>: same as #include<xx.h>
3. Standard header file structure(标准头文件结构)
   ```
   #ifndef HEADER_FLAG  
   #define HEADER_FLAG  
   //Type declaration here  
   #endif //HEADER_FLAG
   ```
4. Tips for header
   1. One class declaration per header file
   2. Associated with one source file in the same prefix of file name(一个头文件不能被include两次)
   3. The contents of a header file is surrrounded with #ifndef #define #endif


