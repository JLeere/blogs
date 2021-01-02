---
title: CXX零碎知识点(二)
date: 2020-10-18 14:59:20
# updateDate: 2020-10-19 18:34:51
categories:
- 编程学习
- C++
tags:
- C++
---

# 2020年11月

## 动态内存

## 条件?=表达式

`int v = (w<0) ? -1:1`表示如果w小于0, v取-1, 否则取1.

## 子类虚函数调用

[参考链接](https://blog.csdn.net/ly890700/article/details/55803398)

覆盖override: 派生类重新写父类中virtual虚函数的实现, 参数列表返回类型需要保持一致.

重载overload: 两个同名函数的参数列表不同, 包括虚函数

> 重定义: 重定义也是描述分别位于父类与子类中的同名函数的，但返回值可以不同。
>
> - 如果参数列表不同，这时子类中重定义的函数不论是否有virtual关键字，都会隐藏父类的同名函数。
> - 如果参数列表相同，但父类中的同名函数没有virtual关键字修饰，此时父类中的函数仍然被隐藏。

<!-- more -->

```C++

#include <iostream>
using namespace std; 

class Base {
public:
    virtual void f(int);
    virtual void f();//父类中定义了两个重载函数f
};

void Base::f(int a) {
    cout << "Base::f(int) " << a << endl;
}

void Base::f() {
    cout << "Base::f() " << endl;
}
//===============================
class Derived:public Base {
public:
    virtual void f(float); //子类重定义了函数f
};

void Derived::f(float a) {
    cout << "Derived::f(float)" << showpoint << a << endl;
}

int main() {
    Base *p1 = new Base;
    p1->f(1);
    
    Derived *p2 = new Derived;//定义指向子类的指针p2
    p2->f(2.0); //调用子类::f(float),可运行 输出Derived::f(float)2.00000
    p2->f(3);// 调用子类::f(float), 可运行输出Derived::f(float)3.00000
    
    Base *p3 = new Derived; //定义指向父类的指针p3,实际指向子类,仍会调用父类的函数f(int)
    p3->f(2.0);//由于重定义(重载)的Derived::f(float)并没有覆盖掉父类的f()函数, 因此会调用父类::f(int), 输出Base::f(int) 2
    p3->f(1);//可运行
    
    delete p1;
    delete p2;
    delete p3;
    
    return 0;
}
```

因此在写子类的虚函数的时候, 如果参数列表不同, 可以不加override关键字, 不用覆盖的功能,而直接使用重定义或者叫重载. 

只是需要注意定义该子类对象时不能是指向父类的指针`Base *p = new Derived`, 否则父类的同名函数不会隐藏,而被调用.

## private / public

**访问范围**:

- private: 只能被该类中的函数、该类的友元函数访问，该类的**实例对象**和**子类函数**都不能访问
- protected成员，可以被该类中的函数、类的友元函数、子类函数访问，该**类的对象**不能访问
- public成员, 可以被类中的函数、友元函数、类的实例对象、子类成员函数**均可访问**.

**继承权限**：

- public：可以访问父类的所有public成员
- protected：父类的public和protected都变成子类的protected
- private：父类的所有成员都变成子类的private

## const引用传参

定义一个类，类中定义`int value()`函数

```C++
class mc_int {
    private: 
        int val;      //actual int
    public: 
        int value();  //Returns value
        int value(int);  //Changes and returns value
        mc_int();  //Default constructor
        mc_int(int);//Create from int
        void asBytes(char*); //generate byte array

        mc_int& operator=(int);
        mc_int& operator=(const mc_int&);

        bool endianity;  //true for little
};
```

当外部定义时采用常引用`mc_int& operator=(const mc_int&);`报错

```C++
mc_int& mc_int::operator=(const mc_int& other) {
            val = other.value();  
            //    |-------->Error: No instance of overloaded function matches the argument list and object (object has type quelifiers that prevent the match)
}
```

原因在于：`other`对象被常引用，意指该对象不会被修改内部成员。而调用的成员函数`other.value()`没有被声明为仅访问函数，因此编译器认为该函数可能会改变常引用的对象`other`而报错。

修改：

```C++
int value() const;//申明为仅访问函数
```

## array vector区别

## 子类构造函数调用父类的构造函数

https://blog.csdn.net/fchyang/article/details/81508030

```C++
ParkingSlot::ParkingSlot(std::vector<PointType2f> rect_points):Object2D(rect_points){}
```

