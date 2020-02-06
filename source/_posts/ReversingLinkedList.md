---
title: 【C】 PTA习题 02-线性结构3 Reversing Linked List
date: 2019-04-10 19:46:53
tags:
    - PAT
    - 链表
    - C
categories: 
    - DataStructure
---

单链表的部分反转  
https://pintia.cn/problem-sets/1077214780527620096/problems/1103507412634030082

<!-- more -->  
### Reversing Linked List  

Given a constant K and a singly linked list L, you are supposed to reverse the links of every K elements on L. For example, given L being 1→2→3→4→5→6, if K=3, then you must output 3→2→1→6→5→4; if K=4, you must output 4→3→2→1→5→6.

Input Specification:
Each input file contains one test case. For each case, the first line contains the address of the first node, a positive N (≤10
​5
​​ ) which is the total number of nodes, and a positive K (≤N) which is the length of the sublist to be reversed. The address of a node is a 5-digit nonnegative integer, and NULL is represented by -1.

Then N lines follow, each describes a node in the format:

Address Data Next
where Address is the position of the node, Data is an integer, and Next is the position of the next node.

Output Specification:
For each case, output the resulting ordered linked list. Each node occupies a line, and is printed in the same format as in the input.

```
Sample Input:  
00100 6 4  
00000 4 99999  
00100 1 12309  
68237 6 -1  
33218 3 00000  
99999 5 68237  
12309 2 33218 

Sample Output:  
00000 4 33218  
33218 3 12309  
12309 2 00100  
00100 1 99999  
99999 5 68237  
68237 6 -1    
```

---

单链表的部分反转，并且存储单链表的方式非常特别。  
参考了两篇博客：  
    https://www.cnblogs.com/kevin-lwb/p/4283456.html  
    https://www.cnblogs.com/byrhuangqiang/p/4311336.html  
详细注释见代码  

### 代码
```C
#include<stdio.h>
#include <malloc.h>
#define MAX_SIZE 100000

typedef struct LinkedList {
    int addr;
    int data;
    int nextAddr;
    struct LinkedList *next;
}List;

/*  反转部分单链表
    返回反转后链表的第一个节点，非头结点
*/
List *listReverse(List *head, int y) {
    int count = 1;
    /*  新建链表，头结点插入法  
        新建一个头结点，遍历原链表，把每个节点用头结点插入到新建链表中。
        最后，新建的链表就是反转后的链表。*/
    List *new = head->next;
    List *cur = new->next;  //cur是要插入新链表的节点
    List *temp = NULL;      //temp,临时保存cur的next

    while(count < y) {
        temp = cur->next;   //temp保存下一次要插入的节点
        cur->next = new;    //把cur插入到new中
        cur->nextAddr = new->addr;  
        new = cur;       //纠正头结点new的指向
        cur = temp;     //cur指向下一次要插入的节点
        count++;
    }
    //使反转后的最后一个节点指向下一段子链表的第一个节点
    head->next->next = cur;
    if(cur != NULL) {
        //修改Next值，使它指向下一个节点的位置
        head->next->nextAddr = cur->addr;
    } else {
        //如果cur为NULL,即没有下一个子链表，那么反转后的最后一个节点即是整个链表的最后一个节点
        head->next->nextAddr = -1;
    }
    return new;
}

void printList(List *a) {
    List *p = a;
    while (p->next !=NULL) {
        p = p->next;
        if(p->nextAddr != -1) {
            //%.5d,如果一个整数不足5位，输出时前面补0
            printf("%.5d %d %.5d\n", p->addr, p->data, p->nextAddr);
        }
        else {
            //-1前面不需要补零
            printf("%.5d %d %d\n", p->addr, p->data, p->nextAddr);
        }
    }
}

int main()
{
    int start, x, y;
    x = 0, y = 0;
    int num = 0;    //链表节点数
    int data[MAX_SIZE]; //存data值，节点位置作为索引值
    int next[MAX_SIZE]; //存next值，节点位置作为索引值
    int temp;

    //输入
    if(scanf("%d %d %d", &start, &x, &y)){
    };
    List a[x + 1];
    a[0].nextAddr = start;
    /*  在输入时，分别将输入的data和nextAddr存放在两个数组中
        利用输入的addr作为索引值
        最后再根据输入构造单链表，使单链表能够顺序存储
        避免了根据addr作为索引时，查找不方便
    */
    while (x--) {
        if(scanf("%d", &temp)){
        };
        if(scanf("%d %d", &data[temp], &next[temp])){
        };
    }

    //构建单链表
    int i = 1;
    while(1){
        if(a[i-1].nextAddr == -1){
            a[i-1].next = NULL;
            num = i - 1;
            break;
        }
        //当前addr = 上一个的 nextAddr
        a[i].addr = a[i - 1].nextAddr;
        //当前的数据data--用a[i].addr(当前addr)作为索引，在data[???]中查找
        a[i].data = data[a[i].addr];
        //当前nextAddr--用a[i].addr(当前addr)作为索引，在next[???]中查找
        a[i].nextAddr = next[a[i].addr];

        a[i-1].next = a + i;

        i++;
    }

    //反转
    List *p = a;
    List *rp = NULL;
    int j = 0;
    if(y <= num)
    {
        //如果链表节点数num能被y整除，则每段都要反转
        for (j = 0; j < (num / y); j++) {
            rp = listReverse(p, y);
            p->next = rp;   // 第一次执行，a[0]->next 指向第一段子链表反转的第一个节点
            p->nextAddr = rp->addr; // 更改Next值，指向逆转后它的下一个节点的位置

            int k = 0;
            //使p指向下一段需要反转的子链表的头结点
            while (k < y) {
                p = p->next;
                k++;
            }
        }
    }

    //输出
    printList(a);

    return 0;
}
```
