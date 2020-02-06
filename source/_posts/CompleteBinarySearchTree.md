---
title: 【C】PTA 习题04-树6 Complete Binary Search Tree
date: 2019-05-03 18:34:45
tags:
    - PAT
    - C
    - 树
categories:
    - DataStructure
---
中序的完全二叉树层序遍历输出
https://pintia.cn/problem-sets/1077214780527620096/problems/1108407010212667394

<!--more-->
## Complete Binary Search Tree
A Binary Search Tree (BST) is recursively defined as a binary tree which has the following properties:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
Both the left and right subtrees must also be binary search trees.
A Complete Binary Tree (CBT) is a tree that is completely filled, with the possible exception of the bottom level, which is filled from left to right.

Now given a sequence of distinct non-negative integer keys, a unique BST can be constructed if it is required that the tree must also be a CBT. You are supposed to output the level order traversal sequence of this BST.

Input Specification:
Each input file contains one test case. For each case, the first line contains a positive integer N (≤1000). Then N distinct non-negative integer keys are given in the next line. All the numbers in a line are separated by a space and are no greater than 2000.

Output Specification:
For each test case, print in one line the level order traversal sequence of the corresponding complete binary search tree. All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line.

```
Sample Input:  
10  
1 2 3 4 5 6 7 8 9 0  
```
```
Sample Output:  
6 3 8 1 5 7 9 0 2 4  
```

## 分析
1. 输入一串数字
2. 将数字进行排序（因为时二叉搜索树，需要排序）
3. 用排序结果构建一个中序遍历的完全二叉树
4. 将完全二叉树层序遍历输出  
   
和Tree Traversals Again有点像。  
参考了一个大佬的博客。因为输入输出都是一串数字，所以没有构建树，根据其关联、规律直接找到层序遍历时的下标，得到这个数组。  
从i=1开始，i * 2为左儿子的下标，i * 2 + 1为右儿子的下标。  
参考博客：http://ytwzju.com/2018/07/26/PAT%20Complete%20Binary%20Search%20Tree/  
常规方法需要注意，中序遍历生成树之前，先生成了一个完全二叉树，然后将输入的数据填入树中。  
https://blog.csdn.net/hsapyzx/article/details/52958006  

## 代码
```C
#include<stdio.h>
#include<stdlib.h>
#define MAX_SIZE 10000

int tree[MAX_SIZE];
int input[MAX_SIZE];
int idx = 0;

//中序遍历满二叉树（唯一确定），进行层次遍历--的规律
//从i=1开始，i*2为左儿子的下标，i*2+1为右儿子的下标
void BuildTree(int root, int size) {
    if(root>size)
        return;
    BuildTree(root * 2, size);
    tree[root] = input[idx++];
    printf("tree[root]-tree[%d]:num-%d\n", root, input[idx]);
    BuildTree(root * 2 + 1, size);
}


int main() {
    int N;
    int i,j;

    scanf("%d", &N);
    for (i = 0; i < N; i++) {
        scanf("%d", &input[i]);
    }

    //冒泡排序
    for (i = 0; i < N; i++) {
        for (j = i; j < N; j++) {
            if(input[i]>input[j]) {
                int temp = input[i];
                input[i] = input[j];
                input[j] = temp;
            }
        }
    }

    //得到层序遍历树
    //注意从1开始
    BuildTree(1, N);
    for (i = 1; i < N+1; i++) {
        printf("%d", tree[i]);        
            if(i!= N)
            printf(" ");
    }
    return 0;
}
```