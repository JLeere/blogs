---
title: C++ 知识点
date: 2020-05-14 22:47:00
categories:
- 编程学习
- C++
tags:
- C++
---

# [源文件和头文件](https://www.cnblogs.com/xxcn/p/10930105.html)

.cpp文件通过`#include .h`直接一字不差的引用头文件中的代码.头文件中,存在变量或者函数的**声明**，而不要放定义.这些全局变量或函数在其他的.cpp源文件中得到定义.因此头文件的作用就是声明即将用的函数或变量,将不同.cpp链接起来,同样的东西便不用在多个cpp中都进行编译.在多个文件中出现了对于一个符号（变量或函数）的定义，纵然这些定义都是相同的，但对于编译器来说，这样做不合法,即不能多重定义
头文件中可以定义的类别,例外:



# [哈希表unordered_map](https://www.jianshu.com/p/4e64fce04a38)

无序映射表，key-value，可以将查询时间复杂度降为o(1).

相关函数查一查。

```c++
//宝石与石头问题
int numJewelsInStones(string J, string S) {
    {
        unordered_map<char, bool> mp;//<Key T>
        int sum = 0;
        for (auto j : J) //j为字符串J中的一个字符
            mp[j] = 1;	//mp[Key] = value
        for (auto s : S)
            if (mp[s]) sum++; //mp[s]有这个Key为1
        return sum;
    }
```

# [申明和定义](https://www.cnblogs.com/frankfang/archive/2011/05/02/2034393.html)

> "定义"的严谨C++语意，即内存占有，编译器将在相对内存地址上为其对象定址！

> 所以有回复说：
>
> 变量和对象不加extern永远是定义,类中的除外。
> 函数只有函数头是声明，有函数体的是定义。
> 类永远只是声明。类成员函数的函数体是定义。

```C++
class MyClass
{
    static int x; //这里的x是声明
    static const int a; //这里的a是声明
    //非static变量在类实例化时才分配内存.
    MyClass();//这里的函数是声明
};
int MyClass::x;//这是定义
const int MyClass::a=11;//这是定义
```



# boost::make_shared

智能指针,可以允许多个指针指向同一个地址内存,并且在所有指针的计数器消除为0时候自动删除内存.

```C++
int* p = new int(233);
boost::shared_ptr<int> sp(p);
//然后就不能再delete p了, 否则重复释放
```

# auto 关键字

声明变量时根据初始化表达式自动推断该变量的类型

`auto z("hello");//const char*`

但是不建议对于简单的变量使用auto,而多用于类型冗长/变量使用范围专一时候使用,方便程序阅读.

```C++
 std::vector<int> vect; 
 for(auto it = vect.begin(); it != vect.end(); ++it)
 {  //it的类型是std::vector<int>::iterator
    std::cin >> *it;
  }
```

# std::transform

# lambda表达式

# 数据类型

sizeof(short) < sizeof(int) < sizeof(long),

short 是2字节，16位；int是32位，4字节；long在32位计算器中也是4字节。

C++中用INT_MAX和INT_MIN可以分别代表int型上下限，（-2^31,2^31-1)

# Boost::shared_ptr

​	