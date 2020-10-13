---
title: C语言 Notes
date: 2019-05-03 09:29:54
tags:
    - C
categories:
    - Notes
---
# C语言笔记_翁恺

mooc上翁恺老师的C语言程序设计，视频笔记

## 指针
### 数组变量是特殊指针
1. 数组变量本身表达地址  
   数组单元表达的是变量
2. []、*运算符，可以对指针、数组做
3. 数组变量是const指针，所以不能被赋值
    `int a[]    <==>   int *const a = `

### 指针与const
1. 指针是const
    不能再指向别的变量
   ```C
    int *const q = &i;  //q是const
    *q = 26;    //ok
    q++;    //error
   ```
2. 所指是const
    不能通过指针去修改这个变量
    ```C
    const int *p = &i;
    *p = 26;    //error,(*p)是const
    i = 26;     //ok
    p = &j;     //ok
    ```
    **/*判断const标准*：const在*的前面还是后面**

### malloc,动态内存分配
    #include<stdlib.h>
    void* malloc(size_t size)
1. 向malloc申请的空间的大小是以字节为单位
2. 返回的结果时void*，需要类型转换为自己需要的类型
3. (int*)malloc(n*sizeof(int))
```C
#include <stdlib.h>

typedef struct AVLNode *Position;
typedef Position AVLTree; /* AVL树类型 */
struct AVLNode
{
    int Data; /* 结点数据 */
    AVLTree Left;     /* 指向左子树 */
    AVLTree Right;    /* 指向右子树 */
    int Height;       /* 树高 */
};

int main() {
    AVLTree R = (AVLTree)malloc(sizeof(struct AVLNode));
}
```


## 结构类型
### typedef
声明一个已有的数据类型的新名字。  
```C
typedef int length
//int:原来的类型名字  
//length:现在的类型名字

typedef struct node {
    int data;
    struct node *next;
}aNode;
//or
typedef struct node aNode;
}
```

## 全局变量
### 全局变量
1. 不初始化，全局变量=0，指针=NULL
2. 初始化发生在main之前

### 静态本地变量
1. 静态本地变量，在本地变量定义前加上static
2. 函数离开时，静态本地变量继续存在，并保存其值
3. 只在第一次初始化
4. 是特殊的**全局变量**，位于相同的内存区域
5. 有全局的生存期，函数内的局部作用域

### 头文件
1. `#include`编译预处理指令
2. 它把那个文件的全部文本内容原封不动地插入到它所在的地方
3. `""`,现在当前目录找，然后在编译器指定目录找
4. `<>`,只在指定的目录找
5. 在**函数**或者**全局变量**前，加上static，则其只能在**当前**编译单元中使用

### 声明
    int i; //是变量的定义
    extern int i;   //是变量的声明
1. 声明，不产生代码
   1. 函数原型
   2. 变量声明
   3. 结构声明
   4. 宏声明
   5. 美剧生命
   6. 类型声明
   7. inline函数
2. 定义，产生代码
3. 只有声明可以放在头文件中
#### 标准头文件结构
防止头文件的重复定义

    #ifndef _LIST_HEAD_
    #define _LIST_HEAD_

    extern int i;

    #endif