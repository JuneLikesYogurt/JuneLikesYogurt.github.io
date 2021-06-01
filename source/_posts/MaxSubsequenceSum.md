---
title: 【C】 PAT 1007 -- Maximum Subsequence Sum
date: 2019-04-01 20:50:11
tags:
    - PAT
    - C
categories: 
    - 刷题
---
最大子列和--在线处理算法  
输出子列和    
参考：  
https://pintia.cn/problem-sets/994805342720868352/problems/994805514284679168
https://pintia.cn/problem-sets/1077214780527620096/problems/1077218398207094785

<!-- more -->
### Maximum Subsequence Sum

Given a sequence of K integers { N
​1
​​ , N
​2
​​ , ..., N
​K
​​  }. A continuous subsequence is defined to be { N
​i
​​ , N
​i+1
​​ , ..., N
​j
​​  } where 1≤i≤j≤K. The Maximum Subsequence is the continuous subsequence which has the largest sum of its elements. For example, given sequence { -2, 11, -4, 13, -5, -2 }, its maximum subsequence is { 11, -4, 13 } with the largest sum being 20.

Now you are supposed to find the largest sum, together with the first and the last numbers of the maximum subsequence.

Input Specification:
Each input file contains one test case. Each case occupies two lines. The first line contains a positive integer K (≤10000). The second line contains K numbers, separated by a space.

Output Specification:
For each test case, output in one line the largest sum, together with the first and the last numbers of the maximum subsequence. The numbers must be separated by one space, but there must be no extra space at the end of a line. In case that the maximum subsequence is not unique, output the one with the smallest indices i and j (as shown by the sample case). If all the K numbers are negative, then its maximum sum is defined to be 0, and you are supposed to output the first and the last numbers of the whole sequence.

```
Sample Input:  
10  
-10 1 2 3 4 -5 -23 3 7 -21  
Sample Output:  
10 1 4  
```
  
---

折腾了好久，最大子列和的进阶，琢磨怎么找最大子列和的首项和尾项就琢磨了好久。其实非常简单。
以下附上最大子列和的代码  

### 最大子列和（返回最大子列和）
```C
#include <stdio.h>
//最大子列和-在线处理
int Count(int a[], int n)
{
	int i;
	int ThisSum = 0, MaxSum = 0;
	for (i = 0; i < n; i++)
	{
		ThisSum += a[i];
		if (ThisSum > MaxSum)
		{
			MaxSum = ThisSum;
		}
		else if (ThisSum <= 0)
		{
			ThisSum = 0;
		}
	}
	return MaxSum;
	//T(N) = O(N)
}

int main()
{
	int n, a[100000];
	int MaxSum;
	if(scanf("%d", &n)){};//防止scanf返回值warning
	//输入数据
	int i;
	for (i = 0; i < n; i++)
	{
		if(scanf("%d", &a[i])){};
	}
	MaxSum = Count(a, n);
	printf("%d", MaxSum);
	return 0;
}

```

### 最大子列和（返回最大子列）

然后PAT这道题比较坑的在于，考虑全负数（题目明确说了要考虑）和只有负数和0的情况。  
注释都在代码里了。  

```C
#include <stdio.h>
//最大子列和-在线处理
int Count(int a[], int n)
{
	int i;
	int ThisSum = 0, MaxSum = 0;

	int tempFirst = 0;
	int first = 0, last = n - 1; //如果存在负数，需要输出子列首、尾的情况
	int flag = 0;	//子列中是否含0

	for (i = 0; i < n; i++)
	{
		ThisSum += a[i];
		if(a[i] == 0) 
		{
			flag = 1;
		}
		//thiSum>maxSum,此时first last为最大子列和的首部和尾部
		if (ThisSum > MaxSum)
		{
			MaxSum = ThisSum;
			first = tempFirst;
			last = i;
		}
		if (ThisSum < 0)
		//！！！，<=0会忽略最大子列和前面有一串0的情况
		{
			//ThisSum<0 重置first
			tempFirst = i + 1;
			ThisSum = 0;
		}
	}

	//输出
	if (MaxSum <= 0)
	{
		//当子列中只有0和负数
		/*有0和负数单独列出的原因：
			最后一个子列为负数时，输出“0 0 x”。而只有0和负数时，要求输出“0 0 0”
		*/
		if (flag == 1) {
		printf("%d %d %d", 0, 0, 0);
		}
		//当子列中只有负数
		else {
		printf("%d %d %d", 0, a[0], a[n - 1]);
		}
	}
	else
	{	
		printf("%d %d %d", MaxSum, a[first], a[last]);
	}
	return 0;
	//T(N) = O(N)
}

int main()
{
	int n, a[10000];
	if (scanf("%d", &n)){}; //防止scanf返回值warning

	//输入数据
	int i;
	for (i = 0; i < n; i++)
	{
		if (scanf("%d", &a[i])){};
	}
	Count(a, n);
	return 0;
}

```