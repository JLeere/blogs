---
title: [教材]C++基础巩固
date: 2020-09-11 23:23:16
categories:
- 编程学习
- C++
tags:
- C++
---

对于在教材和课程学习过程中记录和巩固的基础知识.

# C对比回顾

## 引用

- `int & a = x;`只能对**变量**进行引用，并且后续一直对该变量x保持引用。相当于变量x的别名，改变a的值即改变了x的值。
- 作为**函数参数**时, 可帮助函数传递实参，改变形参即可改变实参的值
- **常引用**: `const int & b = y;` 后续不能改变其值; 也不能将常引用用于初始化非常引用.

## const

- **定义**: `const string Str = "China";` `const int * p = & x`
- **常量指针p**:
  - 不能通过p修改指向内容的值, 因此常量指针作为函数参数时可防止其指向的变量被函数内部修改;
  - p所指向的变量的地址可以改变为其他变量, `p = & y;`
  - 常量指针不能被赋值为非常量指针,反之可以;

## 动态内存分配 new

**定义**: `int * p = new int;` `int * pn = new int[10]`,初始化时就分配好地址空间.

**使用new就必须使用`delete`释放空间**: `delete p` `delete [] p`

## 内联函数, 重载, 缺省

- 内联函数`inline int Max(int a, int b){}`: 函数体比较简单时,为了节约函数调用的**时间**,采用内联在编译时直接将函数体代码复制过去, 不再调用.
- 函数重载: `int Max(double a,double b); int Max( int a, int b); int Max(int a, int b, int c)`:不同的参数列表用同样的函数名
- 缺省参数:` int Max(int a, int b=2, int c = 3)`调用函数时后两个参数可以省略不写

<!-- more-->

# 数据结构

整形int: short 2字节，long 4字节；

实形float（4），double（8）；

bool (1)；char(1)；void;

## 指针

```C++
int x, *p = &x ;//*只是申明p为指针变量，&表示取地址
int y,*q;q=&y; //与上一行等价

int * k = new int(10);//初始化指针变量
int * k = new int[10];//动态数组
delete(k);//释放存储空间
```

## 字符串的输入输出

字符数组,`char s[] = "China"`;

字符指针`char *p = "china"; *q = s`;

字符指针数组`char * book[] = {"C", "python","Matlab"}`

#### 输入流

```C++
// 从终端接收字符：
char arr[5];
string str;

cin>>arr;	//空格，tab，enter都能结束
cin.get(arr,5);	
cin.getline(arr,5);		//取5个字符，包含'\0',可收取空格
getline(cin,str);	//
gets(arr)；	//新标准有用gets_s()
getchar(arr)；	//后三个需`#include <string>
scanf("%d%d",&x,&y); 
```

[详细参见1](https://www.cnblogs.com/rever/p/4360826.html) [参见2](https://blog.csdn.net/u011486738/article/details/82082405)

**scanf和printf不能直接作用于string**，即`scanf("%s", arr)`

#### 输出打印

```C++
cout<<endl;
puts(arr);
printf("%.3f",3.14156);		//"stdio.h"
```

#### printf修饰符


| 符号 | 含义 |
| - | - |
| d或i | 十进制整数 |
| f | 浮点数 |
| s | 字符串 |
| c | 字符 |
| .3 | 宽度 |
