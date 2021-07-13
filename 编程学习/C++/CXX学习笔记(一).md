---
title: CXX学习笔记(一)
date: 2020-06-28 21:00:14
categories:
- 编程学习
- C++
tags:
- C++
---



# 类和对象

## 友元函数friends

## this指针

每个成员函数通过this指针指向自己这个对象/类. 把搭建的类/对象理解成一栋房子,我们在屋内时候不能看到整个房屋,但是this就相当于房子的门牌号, 代替自己房子的地址.

往往自己在些类的时候,正在构建的类没有具体的对象,但需要用到其他成员函数的时候,可以用this->function调用.

```C++
class rect
{
    public:
        rect(int h, int w)
        {
            hight = h;
            width = w;
        }
    	int aera()
        {
            return hight*width;
        }
    	bool compare(rect B)
        {
            return this->aera > B.aera();
        }
    private:
    	int hight;
    	int width;
}
```



# 继承

`class A : public B`

A为派生类(子类)derive, B为基类(父类)base

A类公有继承B类(public), 即A类可以访问B类所有公有和保护的成员(public,protect),但不能访问B类的私有成员.

## [虚函数和纯虚函数](https://www.runoob.com/w3cnote/cpp-virtual-functions.html)

**虚函数**: 基类函数最前面添加`virtual`指令, 父类中定义了虚函数的一种实现方式,子类中可以存在与之同名同参数同返回值的成员函数,并给出*不同的实现*.

```C++
class A
{
public:
    // 虚函数允许派生类不同的实现
    virtual void foo()
    {
        cout<<"A::foo() is called"<<endl;
    }
};
class B:public A
{
public:
    void foo()
    {
        cout<<"B::foo() is called"<<endl;
    }
};
int main(void)
{
    A *a = new B();
    a->foo();   // 在这里，a虽然是指向A的指针，但是被调用的函数(foo)却是B的!因此输出显示"B::foo() is called"
    return 0;
}
```

**纯虚函数**: 当父类是一种抽象类,比如动物,是无法定义一个具体的对象实例的,因此不能得到其animalType. 所以定义一个纯虚函数,让子类(比如大象)自己去定义编写.

```C++
virtual void animalType() = 0
```

作用就是规范类的接口.

*注意的是有纯虚函数定义的类是不能初始化实例对象的.而虚函数是必须要有定义的,否则会报错.*

# 编译预处理

## 头文件

```C++
#include <iostream>
	cin>>;cout<<endl;

#include <stdio>
	printf();
	scanf("x=%d",&x); //从键盘读取数据付给地址内的变量x的，&取地址
#include <string>	
	gets();
	getline();//相比与CIN,此函数只以回车为结束.
	puts();

#include <math>
	cos();exp();fabs();log();pow();sqrt();


```

头文件用双引号 表示优先在当前文件夹目录下寻找；尖括号表示直接去系统指定文件夹寻找。

## 宏定义

```C++
#difine PI 3.141592653
```

## [条件编译](https://www.cnblogs.com/challenger-vip/p/3386819.html)

`#ifndef，#define，#endif`

ifndef: if not define. 实例：

```C++
#ifndef x                 //先测试x是否被宏定义过
#define x
   程序段1blabla~    //如果x没有被宏定义过，定义x，并编译程序段 1
#endif   
　　程序段2blabla~　　 //如果x已经定义过了则编译程序段2的语句，“忽视”程序段 1
```

主要目的是防止头文件的重复编译和包含。避免多个cpp文件都包含同一个h文件时，全局变量被重定义的错误。

# C++11

## `=default，=delete`

[参考链接](https://www.cnblogs.com/lsgxeva/p/7787438.html)

四类特殊的成员函数：构造函数、析构函数、拷贝构造函数、拷贝赋值函数。负责类的对象的创建、初始化、销毁、和拷贝。

C++11 标准引入了一个新特性："=default"函数。程序员只需在函数声明后加上“=default;”，就可将该函数声明为 "=default"函数，编译器将为显式声明的 "=default"函数自动生成函数体。

```C++
// "=default"函数既可以在类体里（inline）定义，也可以在类体外（out-of-line）定义。
class X2
{
public:
    X2() = default; //Inline defaulted 默认构造函数
    X2(const X&);
    X2& operator = (const X&);
    ~X2() = default;  //Inline defaulted 析构函数
};

X2::X2(const X&) = default;  //Out-of-line defaulted 拷贝构造函数
X2& X2::operator= (const X2&) = default;   //Out-of-line defaulted  拷贝赋值操作符


// 为了能够让程序员显式的禁用某个函数，C++11 标准引入了一个新特性："=delete"函数。程序员只需在函数声明后上“=delete;”，就可将该函数禁用。
class X3
{
public:
    X3();
    X3(const X3&) = delete;  // 声明拷贝构造函数为 deleted 函数
    X3& operator = (const X3 &) = delete; // 声明拷贝赋值操作符为 deleted 函数
};
```

## #include<functional>

[链接一](https://www.jianshu.com/p/f191e88dcc80)

[链接二](https://blog.csdn.net/shuilan0066/article/details/82788954)

### std::function

> - 定义格式：std::function<函数返回值类型（函数输入参数类型）>。
>
> - std::function可以取代函数指针的作用，因为它可以延迟函数的执行，特别适合作为回调函数使用。它比普通函数指针更加的灵活和便利。

它是一类函数的模板，给具有相同输入输出类型的函数或方法重命名，需要传入这个函数，同时需要说明<返回值(输入参数)>，如下：

```C++
std::function<int(int,int)> f = functionname_add();
```

对于类成员函数的替代如下：

```C++
class Computer
{
public:
	static int Add(int i, int j)
	{
		return i + j;
	}
 
	template<class T>
	static T AddT(T i, T j)
	{
		return i + j;
	}
 
	int AddN(int i, int j)
	{
		return i + j;
	}
};
// 这几个函数都是int(int,int)类型
int main()
{
	//1、 类静态函数
	std::function<int(int, int)> f = &Computer::Add;
	cout << f(1, 1) << endl;
 
	//2、 类静态模板函数
	std::function<int(int, int)> ft = &Computer::AddT<int>;
	cout << ft(1, 1) << endl;
 
	//成员函数绑定  需要构造类对象
	Computer c;
 
	//3、 成员函数 需使用bind,将类对象地址 &c 绑定上
	std::function<int(int, int)> fN = std::bind(&Computer::AddN, &c, placeholders::_1, placeholders::_2);
	cout << fN(1, 1) << endl;
 
 
	//4、普通函数， 也可以这样调用  个人觉得这个比 bind 麻烦，不建议
	std::function <int(const Computer ＆, int, int)> fN2 = &Computer::AddN;
	cout << fN2(c,1, 1) << endl;
 
	getchar();
	return 0;
}
```

### std::bind

参见链接二。

>std::bind将可调用对象与其参数一起进行绑定，绑定后的结果可以使用std::function保存。std::bind主要有以下两个作用：

> - 将可调用对象和其参数绑定成一个防函数；
> - 只绑定部分参数，减少可调用对象传入的参数。

```C++
double my_divide (double x, double y) {return x/y;}
auto fn_half = std::bind (my_divide,_1,2);  
std::cout << fn_half(10) << '\n';               
```

第一个参数是函数，被隐式转换为了函数指针，

第二个参数_1表示占位符，std::placeholders::__1

第三个参数便是输入值double y为2

### std::transform

http://c.biancheng.net/view/623.html

它可以将一个或两个数组中对应的每个元素单独拎出来执行运算。

一元函数情况下包含3个参数：

- 数组
- 输出的存储数组
- 函数/匿名函数

二元函数情况下包括5个参数：

- 第一个变量的数组的开始

- 第一个变量的数组的结束
- 第二个变量的数组
- 输出的保存数组
- 处理函数/匿名函数