---
title: 【C】 PTA习题 02-线性结构4 Pop Sequence
date: 2019-04-10 21:02:46
tags:
    - PAT
    - C
    - 栈
categories:
    - DataStructure
---
输入几串随机数列，来比较判断当前出栈是否合理  
https://pintia.cn/problem-sets/1077214780527620096/problems/1103507412634030083

<!--more -->
### Pop Sequence

Given a stack which can keep M numbers at most. Push N numbers in the order of 1, 2, 3, ..., N and pop randomly. You are supposed to tell if a given sequence of numbers is a possible pop sequence of the stack. For example, if M is 5 and N is 7, we can obtain 1, 2, 3, 4, 5, 6, 7 from the stack, but not 3, 2, 1, 7, 5, 6, 4.

Input Specification:
Each input file contains one test case. For each case, the first line contains 3 numbers (all no more than 1000): M (the maximum capacity of the stack), N (the length of push sequence), and K (the number of pop sequences to be checked). Then K lines follow, each contains a pop sequence of N numbers. All the numbers in a line are separated by a space.

Output Specification:
For each pop sequence, print in one line "YES" if it is indeed a possible pop sequence of the stack, or "NO" if not.

Sample Input:
5 7 5  
1 2 3 4 5 6 7  
3 2 1 7 5 6 4  
7 6 5 4 3 2 1  
5 6 4 3 7 2 1  
1 7 6 5 4 3 2  
Sample Output:  
YES  
NO  
NO  
YES  
NO    

---

### 分析
这道题看题就看了很久，一直没明白到底要干嘛。后来百度了好多别人的博客，都没有讲的很明白的，不过最终还是弄懂啦。  
题目要求(我的理解)：
1. 输入几串随机数列，来比较判断当前出栈是否合理  
2. 如何判断不合理的标准：
   1. 栈非空 且 栈顶元素>将要比较的出栈元素
   2. **或者** 栈满 且 栈顶元素<将要比较的出栈元素
   
然后就是对于栈的操作的熟悉啦，栈的结构、栈的初始化、出栈、入栈。
https://www.cnblogs.com/yudongxuan/p/7727605.html  
详细注释见代码

### PopSequence 代码实现

```C
#include<stdio.h>
#include<stdlib.h>

typedef struct Node {  //c语言栈基本结构的构造
    int *Data;
    int Top;
    int MaxSize;
}*Stack;

int M, N, K;  //全局变量，放在栈结构后，方便子函数使用

Stack CreateStack(int MaxSize) {
    Stack S = (Stack)malloc(sizeof(struct Node));
    S->Data = (int *)malloc(MaxSize * sizeof(int));
    S->Top = -1;
    S->MaxSize = MaxSize;
    return S;
}

void Push(Stack S, int ele) {
    if (S->Top == S->MaxSize) {  //短路写法，遇错就返回，提高程序可读性
        printf("FULL\n");
        return;
    }
    S->Data[++(S->Top)] = ele;
}

void Pop(Stack S) {
    if (S->Top == -1) {
        printf("Empty\n");
        return;
    }
    S->Top--;
}

int top(Stack S) {  //返回栈顶元素
    return S->Data[S->Top];
}

int StackCheck(int *v,Stack S) {  //检查输出序列是否为容量为M的栈输出序列
    int idx = 0;  //数组v的指标，指向下一个将要出栈的元素
    int num = 1;  //按顺序进栈的元素，进栈自动加1
    S->Top = -1;
    while (idx != N) {
        while ( (S->Top != -1 && top(S)<v[idx]&&S->Top+1<M) || 
                (S->Top==-1&&idx!=N)) {
            Push(S, num++);  //若栈非空且栈顶元素小于将要比较的出栈元素或者栈为空且idx未滑至N(即出栈序列未比较完成）
        }
        if (S->Top!=-1&&top(S) == v[idx]) {  //栈非空且栈顶元素值等于将要比较的出栈元素
            Pop(S);
            idx++;
        }
         /*  栈非空 且 栈顶元素>将要比较的出栈元素
            or 栈满 且 栈顶元素<将要比较的出栈元素
            则非法 
        */
        if ((S->Top != -1&&top(S)>v[idx]) || (
            S->Top + 1 == M&&top(S) != v[idx])) {  
            return 0;  
        }
    }

    return 1;
}

int main() {
    int i, j;
    Stack S;

    scanf("%d %d %d", &M, &N, &K);
    int *v = (int *)malloc(N * sizeof(int));
    S = CreateStack(M);
    for (i = 0;i < K;i++) {
        for (j = 0;j < N;j++) {
            scanf("%d", v + j);
        }
        if (StackCheck(v, S))
            printf("YES\n");
        else
            printf("NO\n");
    }
    return 0;
}


```