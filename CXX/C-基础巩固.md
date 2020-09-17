---
title: C++基础巩固
date: 2020-09-11 23:23:16
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

#include <math>
	cos();exp();fabs();log();pow();sqrt();


```

头文件用双引号 表示优先在当前文件夹目录下寻找；尖括号表示直接去系统指定文件夹寻找。

宏定义

```C++
#difine PI 3.141592653
```

## 数据结构

整形int：short 2字节，long 4字节；实形float（4），double（8）；bool（1）；char（1）；void;

### 指针

```C++
int x, *p = &x ;//*只是申明p为指针变量，&表示取地址
int y,*q;q=&y; //与上一行等价

int * k = new int(10);//初始化指针变量
int * k = new int[10];//动态数组
delete(k);//释放存储空间
```



