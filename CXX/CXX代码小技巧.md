---
title: C++ 知识点
date: 2020-05-14 22:47:00
categories:
- 编程学习笔记
tags:
- C++
---



## 自动变量

```c++
auto res = vector<int>() 
```

## 哈希表

```C++
unodered_map<char int> mp;
for(char c:J)
    
```

## -p.x < 18.0f`表示18是浮点数而非int

## emplace

C++11中vector添加emplace_front,emplace和emplace_back，这些操作构造而不是拷贝元素。这些操作分别对应push_front、insert和push_back，允许我们将元素放置在容器头部、一个指定位置之前或容器尾部。可以提高插入运算的效率

## [const](https://www.runoob.com/w3cnote/cpp-const-keyword.html)

```C++
inline const float &Area() const { return _area; }
```

第一个const修饰的是引用的内容为常量不能修改

第二个const修饰的是类成员函数，确保成员函数不修改被调用对象的值_area。如果我们不想修改一个调用对象的值，所有的成员函数都应当声明为 const 成员函数。

# 



