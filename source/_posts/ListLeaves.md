---
title: 【C】 PTA习题 03-树2 List Leaves
date: 2019-04-24 08:48:46
tags:
    - PAT
    - C
    - 树
    - 队列
categories:
    - DataStructure
---

层次遍历，依次输出树的叶子结点  
https://pintia.cn/problem-sets/1077214780527620096/problems/1106167947099181057  

<!-- more -->
### List Leaves

Given a tree, you are supposed to list all the leaves in the order of top down, and left to right.

Input Specification:
Each input file contains one test case. For each case, the first line gives a positive integer N (≤10) which is the total number of nodes in the tree -- and hence the nodes are numbered from 0 to N−1. Then N lines follow, each corresponds to a node, and gives the indices of the left and right children of the node. If the child does not exist, a "-" will be put at the position. Any pair of children are separated by a space.

Output Specification:
For each test case, print in one line all the leaves' indices in the order of top down, and left to right. There must be exactly one space between any adjacent numbers, and no extra space at the end of the line.
```
Sample Input:
8
1 -
- -
0 -
2 7
- -
- -
5 -
4 6
```
```
Sample Output:
4 1 5
```

---

层次遍历，依次输出树的叶子结点  
用队列实现层次遍历，注意队列的指针的使用  
注释详见代码

### 代码
```C
#include <stdio.h>
#define MaxTree 10

struct TreeNode
{
    int left;
    int right;
} T[MaxTree];

int TreeBuild(struct TreeNode T[])
{
    int num;
    int check[MaxTree];
    int i;
    char cl, cr;
    int root = -1;

    if (scanf("%d", &num))
    {
    };
    if (num)
    { //如果不为空树
        for (i = 0; i < num; i++)
            check[i] = 0; //默认大家都没有被指向，置零
        for (i = 0; i < num; i++)
        {
            if (scanf("\n %c %c", &cl, &cr))
            {
            };
            if (cl != '-')
            {
                T[i].left = cl - '0';
                check[T[i].left] = 1;
            }
            else
                T[i].left = -1;
            if (cr != '-')
            {
                T[i].right = cr - '0';
                check[T[i].right] = 1;
            }
            else
                T[i].right = -1;
        }
        //找出根节点
        for (i = 0; i < num; i++)
            if (!check[i])
                break;
        root = i;
    }
    return root;
}

//队列实现层序遍历，找到叶子节点，并输出
void FindLeaves(int root)
{
    int Que[MaxTree];
    int head = 0, rear = 0;   
    int count = 0; //记录输出叶节点个数，使第一个节点输出是没有空格
    Que[rear++] = root;
    /*  rear,head两个指针。rear参见平时用的i（int i= 0,i++)
        而head指针不断后移，使队列的头不断出队
        由于TreeNode T[]并不是顺序存储，所以在一轮判断结束后，
        根据最前队列的值，继续在树中查找，达到层序遍历的效果
    */
    while (rear - head)
    {
        int x = Que[head++];    //队首节点出队
        if ((T[x].left == -1) && (T[x].right == -1))
        {
            if (count)
                printf(" ");
            printf("%d", x);
            ++count;
        }
        if (T[x].left != -1)
        {
            Que[rear++] = T[x].left;
        }
        if (T[x].right != -1)
        {
            Que[rear++] = T[x].right;
        }
    }
}

int main()
{
    int root;
    root = TreeBuild(T);

    FindLeaves(root);

    getchar();
    return 0;
}
```