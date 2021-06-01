---
title: C语言词法分析器
date: 2019-04-15 10:52:22
tags:
    - C
    - 编译原理
categories:
    - campus
---
# 实验一  简单的C语言词法分析器的实现

### 实验目的：  
设计、编制并调试一个简单的c语言词法分析程序，加深对词法分析原理的理解  

### 实验要求： 
1. 对单词的构词规则有明确的定义；
1. 编写的分析程序能够正确识别源程序中的单词符号；
1. 识别出的单词以（单词符号，种别码）的形式保存在符号表中；
2. 词法分析中源程序的输入以.c格式，分析后的符号表，将二元组保存在.txt文件中。  
   
### 实验分析：
1. 待分析的简单的词法  
    1. 关键字  
 if  else  while  do  for  main return int float double char   
所有的关键字都是小写。  
   1. 运算符  
 =  +  -  *  /   %  <  <=   >  >=  !=  = =   
    3. 界限符  
;  (  )  { }  
   1. 常量  
整形常量，通过以下正规式定义：  
dight dight*  
   1. 标识符（ID），通过以下正规式定义：  
 letter (letter | digit)*  
   1. 空格有空白、制表符和换行符组成。空格一般用来分隔标识符、整数、运算符、界符和关键字，词法分析阶段被忽略。  
1. 各种单词符号对应的种别码：  


|        单词符号          | 类别码  | 单词符号 | 类别码 |  
| :---------------------: | :----: | :------: | :----: |
|          main           |   1    |    >=    |   16   |
|           if            |   2    |    <     |   17   |
|          else           |   3    |   < =    |   18   |
|          wile           |   4    |   = =    |   19   |
|           do            |   5    |    !=    |   20   |
|           for           |   6    |    =     |   21   |
|         return          |   7    |    ；    |   22   |
| lettet（letter/digit）* |   8     |    (     |   23   |
|      dight dight*       |   9    |    )     |   24   |
|            +            |   10   |    {     |   25   |
|           —             |   11   |    }     |   26   |
|            *            |   12   |   int    |   27   |
|            /            |   13   |  float   |   28   |
|            %            |   14   |  double  |   29   |
|            >            |   15   |   char   |   30   |


--- 
### 分析：  
1. 只对于单词 数字等进行识别，不需要理解出意思。
2. 在对关键字标识符单词进行识别时，单独申请了一个数组存放这些关键字；并识别这些单词，判断是哪一个标识符or关键字
3. 整型常量也要注意整个数字的完整
4. <= >= != 这种有两个符号的运算符，也要注意完整扫描
5. 词法分析的代码主要参考了：https://www.cnblogs.com/coderkl/p/4320325.html
6. 因为实验还要求文件的读写，遂自己补充了功能。
7. 由于扫描结果输出的结果有int有char型变量，并且有的文件读写函数读一行、读一个单词、字母，写一个字符，等等诸如此类，非常不方便输出。最后一番调试，调用了fstream的头文件，通过"<<"这一字符以类似cout的方式非常方便地输出了结果（本质上还是太菜，对文件读写操作根本不熟
8. 文件读写参考：https://www.cnblogs.com/forcheryl/p/3963550.html

--- 

### 代码
```C++
#include <fstream>
#include <iostream>
#include <string.h>
using namespace std;

char special[11][11] = {
    "main", "if", "else", "while", "do",
    "for", "return", "int", "float", "double", "char"};
char ch;         //输入
char input[100]; //输入的字符串

int flag;       //种别码
int num;        //存放整型值
char token[10]; //单词符号的字符串
int i = 0;      //input计数
int j = 0;  //token计数

void Scanner()
{
    //清空token
    j = 0; //token计数
    for (int a = 0; a < 10; a++)
    {
        token[a] = NULL;
    }

    ch = input[i++];
    if (ch == ' ')
        ch = input[i++];

    //关键字、标识符
    if ((ch >= 'a' && ch <= 'z') || (ch >= 'A' && ch <= 'Z'))
    {
        while ((ch >= 'a' && ch <= 'z') || (ch >= 'A' && ch <= 'Z') || (ch >= '0' && ch <= '9'))
        {
            token[j++] = ch;
            ch = input[i++];
        }
        token[j++] = '\0';
        ch = input[--i]; //--i,获取下一个input(一个标识符识别出来之后会多加一个i)
        flag = 8;        //关键字类别码
        // 11个关键字、标识符
        for (int y = 0; y < 11; y++)
        {
            if (strcmp(token, special[y]) == 0) //main if 标识符类别码
            {
                if (y > 6) //int float 等标识符类别码
                    y += 20;
                flag = y + 1;
            }
        }
    }

    //数字(整型常量)
    else if ((ch >= '0' && ch <= '9'))
    {
        while ((ch >= '0' && ch <= '9'))
        {
            num = num * 10 + ch - '0'; //???????????
            ch = input[i++];
        }
        ch = input[--i];
        flag = 9;
    }

    //运算符、界限符
    else
    {
        switch (ch)
        {
        case '>':
            j = 0;
            token[j++] = ch;
            ch = input[i++];
            if (ch == '=')
            { //>=
                flag = 16;
                token[j++] = ch;
            }
            else
            { // >
                flag = 15;
                ch = input[--i];
            }
            break;
        case '<':
            token[j++] = ch;
            ch = input[i++];
            if (ch == '=')
            { // <=
                flag = 18;
                token[j++] = ch;
            }
            else
            { // <
                flag = 17;
                ch = input[i--];
            }
            break;
        case '=':
            token[j++] = ch;
            ch = input[i++];
            if (ch == '=')
            { // ==
                flag = 19;
                token[j++] = ch;
            }
            else
            { // =
                flag = 21;
                ch = input[i--];
            }
            break;
        case '!': // !=
            token[j++] = ch;
            ch = input[i++];
            token[j++] = ch;
            flag = 20;
            break;
        case '+':
            flag = 10;
            token[0] = ch;
            break;
        case '-':
            flag = 11;
            token[0] = ch;
            break;
        case '*':
            flag = 12;
            token[0] = ch;
            break;
        case '/':
            flag = 13;
            token[0] = ch;
            break;
        case '%':
            flag = 14;
            token[0] = ch;
            break;
        case ';':
            flag = 22;
            token[0] = ch;
            break;
        case '(':
            flag = 23;
            token[0] = ch;
            break;
        case ')':
            flag = 24;
            token[0] = ch;
            break;
        case '{':
            flag = 25;
            token[0] = ch;
            break;
        case '}':
            flag = 26;
            token[0] = ch;
            break;
        case '#':
            flag = 0;
            token[0] = ch;
            break;
        default:
            flag = -1;
        }
    }
}

int main()
{
    // cout << special[0] << endl;

    ifstream OpenFile("test.c");
    ofstream OutFile("test.txt",ios::out);
    if (OpenFile.fail())
    {
        cout << "打开文件错误!" << endl;
        exit(0);
    }

    cout << "read your code from file,end with '#': " << endl;
    do
    {
        // ch = getchar(); //cin会忽略空格，导致标识符识别错误
        OpenFile.get(ch);
        input[i++] = ch;
    } while (ch != '#');
    cout << input << endl;

    i = 0; //i置0，从input的第一个开始scan
    do
    {
        Scanner();
        if (flag == 9) {
            cout << flag << "," << num << endl;
            OutFile << flag << "," << num << endl;
        }
        if (flag == -1){
            cout << "unrecognise" << endl;
            OutFile << "unrecognise" << endl;
        }
        else{
            cout << flag << "," << token << endl;
            // OutFile.put(flag);
            OutFile << flag << "," << token << endl;
        }
    } while (flag != 0);

    OpenFile.close();
    OutFile.close();
    getchar();
    return 0;
}

```