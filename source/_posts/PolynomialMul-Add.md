---
title: 【C】 PTA习题02-线性结构2 一元多项式的乘法与加法运算
date: 2019-04-06 17:10:57
tags:
    - PAT
    - C
    - 链表
categories: 
    - DataStructure
---

设计函数分别求两个一元多项式的乘积与和  
https://pintia.cn/problem-sets/1077214780527620096/problems/1103507412634030081

<!-- more -->  

### 一元多项式的乘法与加法运算
设计函数分别求两个一元多项式的乘积与和。

输入格式:
输入分2行，每行分别先给出多项式非零项的个数，再以指数递降方式输入一个多项式非零项系数和指数（绝对值均为不超过1000的整数）。数字间以空格分隔。

输出格式:
输出分2行，分别以指数递降方式输出乘积多项式以及和多项式非零项的系数和指数。数字间以空格分隔，但结尾不能有多余空格。零多项式应输出0 0。

```
输入样例:  
4 3 4 -5 2  6 1  -2 0  
3 5 20  -7 4  3 1  
输出样例:  
15 24 -25 22 30 21 -10 20 -21 8 35 6 -33 5 14 4 -15 3 18 2 -6 1  
5 20 -4 4 -5 2 9 1 -2 0
```

---

浙大mooc 链表小白专场那里有详细的讲解，和大部分代码。在此基础上完成了这道题。  
注释见代码。  

### 代码
```C
#include <malloc.h>
#include <stdio.h>

typedef struct PolyNode *Polynomial;
struct PolyNode
{
    int coef;  //常数项
    int expon; //幂
    Polynomial next;
};

//读入多项式
void Attach(int coef, int expon, Polynomial *aRear)
{
    Polynomial p;
    p = (Polynomial)malloc(sizeof(struct PolyNode));
    p->coef = coef;
    p->expon = expon;
    p->next = NULL;
    (*aRear)->next = p;
    *aRear = p;
}

Polynomial ReadPoly()
{
    int i, coef, expon;
    Polynomial P, Rear, temp;

    P = (Polynomial)malloc(sizeof(struct PolyNode));
    P->next = NULL;
    Rear = P;

    if (scanf("%d", &i))
    {
    };
    while (i--)
    {
        if (scanf("%d %d", &coef, &expon))
        {
        };
        Attach(coef, expon, &Rear);
    }
    //链表从第二个开始插入，最后返回的时候删除第一个
    temp = P;
    P = P->next;
    free(temp);

    return P;
}

Polynomial Mult(Polynomial p1, Polynomial p2)
{
    //逐项插入
    Polynomial t1, t2, t, P, Rear;
    int c, e;

    if (!p1 || !p2) //p1 p2两个多项式皆为空
        return NULL;

    t1 = p1, t2 = p2;
    P = (Polynomial)malloc(sizeof(struct PolyNode));
    P->next = NULL;
    Rear = P;

    while (t2) //先用p1的第一项乘以p2，得到P
    {
        Attach(t1->coef * t2->coef, t1->expon + t2->expon, &Rear);
        t2 = t2->next;
    }
    t1 = t1->next;
    while (t1)
    {
        t2 = p2;
        Rear = P;
        while (t2)
        {
            e = t1->expon + t2->expon; //幂相加
            c = t1->coef * t2->coef;   //常数项相乘
            //幂高于Rear中的
            while (Rear->next && (Rear->next->expon > e))
                Rear = Rear->next;
            //相同次幂
            if (Rear->next && (Rear->next->expon == e))
            {
                if (Rear->next->coef + c)
                {
                    Rear->next->coef += c;
                }
                else //常数项相加为0
                {
                    t = Rear->next;
                    Rear->next = t->next;
                    free(t);
                }
            }
            //多项式中项的幂低于Rear中的，将Rear中的插入
            else
            {
                t = (Polynomial)malloc(sizeof(struct PolyNode));
                t->coef = c;
                t->expon = e;
                t->next = Rear->next;
                Rear->next = t;
                Rear = Rear->next;
            }
            t2 = t2->next;
        }
        t1 = t1->next;
    }
    t = P;
    P = P->next;
    free(t);

    return P;
}

Polynomial Add(Polynomial p1, Polynomial p2)
{
    Polynomial temp, P, Rear;
    int c;

    //新声明了一个P，用来存放p1 p2的和
    P = (Polynomial)malloc(sizeof(struct PolyNode));
    P->next = NULL;
    Rear = P;

    while (p1 && p2)
    {
        if (p1->expon == p2->expon) //幂指数相同
        {
            c = p1->coef + p2->coef;
            if (c)
            {
                Attach(c, p1->expon, &Rear);
            }
            p1 = p1->next;
            p2 = p2->next;
        }
        else if (p1->expon > p2->expon) //p1幂指数 > p2幂指数
        {
            Attach(p1->coef, p1->expon, &Rear);
            p1 = p1->next;
        }
        else
        {            
            Attach(p2->coef, p2->expon, &Rear);
            p2 = p2->next;
        }
    }
    while (p1)
    {
        Attach(p1->coef, p1->expon, &Rear);
        p1 = p1->next;
    }
    while (p2)
    {
        Attach(p2->coef, p2->expon, &Rear);
        p2 = p2->next;
    }
    temp = P;
    P = P->next;
    free(temp);

    return P;
}

Polynomial PrintPoly(Polynomial P)
{
    int flag = 0;

    if (!P)
    {
        printf("0 0\n");
        return NULL;
    }

    while (P)
    {
        if (!flag)
            flag = 1;
        else
            printf(" ");
        printf("%d %d", P->coef, P->expon);
        P = P->next;
    }
    printf("\n");

    return 0;
}

int main()
{
    Polynomial P1, P2, PP, PS;

    P1 = ReadPoly();
    P2 = ReadPoly();
    // PrintPoly(P1);
    // PrintPoly(P2);
    PP = Mult(P1, P2);
    PrintPoly(PP);
    PS = Add(P1, P2);
    PrintPoly(PS);

    return 0;
}
```