---
title: 【C】 PTA习题 05-树8 File Transfer
date: 2019-05-04 16:22:07
tags:
categories:
    - DataStructure
---
关于并查集的操作，以及并查集算法的优化，按秩归并和路径压缩。  
https://pintia.cn/problem-sets/1077214780527620096/problems/1111280682067111937
<!--more -->

## File Transfer
We have a network of computers and a list of bi-directional connections. Each of these connections allows a file transfer from one computer to another. Is it possible to send a file from any computer on the network to any other?

Input Specification:
Each input file contains one test case. For each test case, the first line contains N (2≤N≤10
​4
​​ ), the total number of computers in a network. Each computer in the network is then represented by a positive integer between 1 and N. Then in the following lines, the input is given in the format:

    I c1 c2  
where I stands for inputting a connection between c1 and c2; or

    C c1 c2    
where C stands for checking if it is possible to transfer files between c1 and c2; or

    S
where S stands for stopping this case.

Output Specification:
For each C case, print in one line the word "yes" or "no" if it is possible or impossible to transfer files between c1 and c2, respectively. At the end of each case, print in one line "The network is connected." if there is a path between any pair of computers; or "There are  k components." where k is the number of connected components in this network.
```
Sample Input 1:
5
C 3 2
I 3 2
C 1 5
I 4 5
I 2 4
C 3 5
S
```
```
Sample Output 1:
no
no
yes
There are 2 components.
```
```
Sample Input 2:
5
C 3 2
I 3 2
C 1 5
I 4 5
I 2 4
C 3 5
I 1 3
C 1 5
S
```
```
Sample Output 2:
no
no
yes
yes
The network is connected.
```

---

并没有分析，陈越姥姥在小白专场里讲的非常好了，基础的部分看一下何老师的集合回顾一下就ok。  
详细注释见代码。


## 代码
```C
#include <stdio.h>
#define MaxSize 100000

/*  简化了集合的结构
    因为数据是1 2 3...有序数字
    将其与下标关联起来，数x存在下标为x-1的地方
*/
typedef int ElementTpye;
typedef int SetName;
typedef ElementTpye SetType[MaxSize];

void Initialization(SetType S, int n) {
    //默认集合元素全部初始化为-1
    while (n--)
        S[n] = -1 ;
}

/*  路径压缩
    防止查找次数非常非常多的时候，每次都要往子节点下面找，影响性能
*/
SetName Find(SetType S, ElementTpye X) {
    if (S[X] < 0)
        return X;
    else
        //找到根，并把根变为X的父节点，再返回根
        //使一条路径下的节点，全部直接指向这个根
        return S[X] = Find(S, S[X]);
}

/*  按秩归并：
        避免直接归并时，树高过高，导致查找时过深
        最坏情况下，树高 = O(logN)
        1. 按树高   把矮树贴到高树上，S[Root] = -树高 
        2. 比规模   把小树贴到大树上，S[Root] = -元素个数
    
*/
void Union(SetType S, SetName Root1, SetName Root2) {
    // 注意S[Root]为负    
    if (S[Root2] > S[Root1]) {
        //注意按规模时，要把两棵树的元素个数加起来
        S[Root2] += S[Root1];
        S[Root1] = Root2;
    }
    else {
        S[Root1] += S[Root2];
        S[Root2] = Root1;
    }
}

void Input_connection(SetType S) {
    ElementTpye u, v;
    SetName Root1, Root2;
    scanf("%d %d\n", &u, &v);
    //简化了集合结构
    Root1 = Find(S, u - 1);
    Root2 = Find(S, v - 1);
    if (Root1 != Root2)
        Union(S, Root1, Root2);
}

void Check_connection (SetType S) {
    ElementTpye u, v;
    SetName Root1, Root2;
    scanf("%d %d\n", &u, &v);
    Root1 = Find(S, u - 1);
    Root2 = Find(S, v - 1);
    if(Root1 != Root2)
        printf("no\n");
    else
        printf("yes\n");
}

void Check_network(SetType S, int n) {
    int i, counter = 0;
    for (i = 0; i < n; i++) 
        if(S[i] < 0)
            counter++;
    if(counter ==1)
        printf("The network is connected.\n");
    else
        printf("There are %d components.\n", counter);
}

int main()
{
    //初始化集合
    SetType S;
    int n;
    char in;
    scanf("%d\n", &n);
    Initialization(S, n);

    do
    {
        scanf("%c", &in);
        switch (in)
        {
            //Union(Find)
            case 'I':
            {
                Input_connection(S);
                break;
            }
            //Find
            case 'C':
            {
                Check_connection(S);
                break;
            }
            //数集合的根
            case 'S':
            {
                Check_network(S, n);
                break;
            }
        }
    } while (in != 'S');

    return 0;
}
```