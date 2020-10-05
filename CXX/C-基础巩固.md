---
title: C++基础巩固(一)
date: 2020-09-11 23:23:16
categories:
- 编程学习
- C++
tags:
- C++
---

## 编译预处理

```C++
#include <iostream>
	cin>>;cout<<endl;

#include <stdio>
	printf();
	scanf("x=%d",&x); //从键盘读取数据付给地址内的变量x的，&取地址
#include <string>	
gets();getline();//相比与CIN,此函数只以回车为结束.
	puts();

#include <math>
	cos();exp();fabs();log();pow();sqrt();


```

头文件用双引号 表示优先在当前文件夹目录下寻找；尖括号表示直接去系统指定文件夹寻找。

## 宏定义

```C++
#difine PI 3.141592653
```

## 数据结构

整形int: short 2字节，long 4字节；

实形float（4），double（8）；

bool (1)；char(1)；void;

### 指针

```C++
int x, *p = &x ;//*只是申明p为指针变量，&表示取地址
int y,*q;q=&y; //与上一行等价

int * k = new int(10);//初始化指针变量
int * k = new int[10];//动态数组
delete(k);//释放存储空间
```

### 字符串的输入输出

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

| 符号 | 含义       |
| ---- | ---------- |
| d或i | 十进制整数 |
| f    | 浮点数     |
| s    | 字符串     |
| c    | 字符       |
| .3   | 宽度       |









