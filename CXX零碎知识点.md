---
title: C++知识点学习笔记(一)
date: 2020-05-14 22:47:00
categories:
- 编程学习
- C++
tags:
- C++
---

# 2020年5月

## [源文件和头文件](https://www.cnblogs.com/xxcn/p/10930105.html)

.cpp文件通过`#include .h`直接一字不差的引用头文件中的代码.头文件中,存在变量或者函数的**声明**，而不要放定义.这些全局变量或函数在其他的.cpp源文件中得到定义.因此头文件的作用就是声明即将用的函数或变量,将不同.cpp链接起来,同样的东西便不用在多个cpp中都进行编译.在多个文件中出现了对于一个符号（变量或函数）的定义，纵然这些定义都是相同的，但对于编译器来说，这样做不合法,即不能多重定义
头文件中可以定义的类别,例外:



## [哈希表unordered_map](https://www.jianshu.com/p/4e64fce04a38)

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

## [声明和定义](https://www.cnblogs.com/frankfang/archive/2011/05/02/2034393.html)

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



## boost::make_shared

智能指针,可以允许多个指针指向同一个地址内存,并且在所有指针的计数器消除为0时候自动删除内存.

```C++
int* p = new int(233);
boost::shared_ptr<int> sp(p);
//然后就不能再delete p了, 否则重复释放
```

## auto 关键字

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

# 2020年9月

## lambda表达式

```C++
int x = 0;
auto b = [](int x){return x *= 2;};
auto c = [](int x){cout << x <<endl;}(123);//函数体后的123也可传递参数x
```

一般是以上形式: 

- 方括号[]开始, 用于捕捉外部参数变量;
- 圆括号()类似函数体的输入参数
- 花括号{} 是函数主体

```C++
Int x;
auto c = [x](){return x += 3;}; //复制传值,不会改变外部变量x的值
auto c = [&x](){return x += 3;}; //引用传递,可改变
auto c = [=](){return x += 3;};
auto c = [&](){return x += 3;}; //隐形传递, =表示值传递,&表示引用
```

完全格式:

```C++
auto d = [](int a,int b)->int{return a+b;}();// ->后表示函数体的返回值
```

## std::transform

## sizeof

sizeof(short) < sizeof(int) < sizeof(long),

short 是2字节，16位；int是32位，4字节；long在32位计算器中也是4字节。

C++中用INT_MAX和INT_MIN可以分别代表int型上下限，（-2^31,2^31-1)

## Boost::shared_ptr

## 其他

```C++
cloud-> points.swap(filtered_points);
```

## 运算符重载 operator=

```C++
IntCell & operator = (const IntCell & rhs)
{
    if(this != &rhs)
        *membervalue  = *rhs.membervalue;
    return *this;
}
```

在拷贝复制自定义对象的时候, `myclass a = b;`, 等号`=`的运算逻辑需要重载, 因此引入`operator=`. 应当将二者一起看做一个函数, 就方便理解了. 

# 2020年10月

## static关键字

### 静态声明

早起使用static声明变量, 使得其在整个文件中都有效, 如今已经被命名空间取代.

### 类的静态成员

- 静态成员变量或静态成员函数是面向这一大类的, 而非具体实例. 比如人这一个class, 其平均年龄或者寿命属性是针对这一类人, 而不像名字年龄这种针对个体的, 可定义为static. 它能被所有对象共享.

  ```C++
  class human
  {
      static int avgLife(){};
      ……
  ```

- 可通过类的对象,引用,指针访问静态成员

  ````C++
  int old = human::avgLife();
  human a1;
  human * a2 = &a1;
  old = a1.avgLife();
  old = a2->avgLife();
  ````

- 成员函数可直接调用静态成员

- 静态成员函数不能调用非静态成员，内部实现不能用this指针，也不能和仅访问函数const一起使用

## 多态 vitual/override/final

**vitual**: 定义虚函数, 允许不同的派生类使用相同名字的函数进行分别不同的实现.

**vitual =0**有时候基类并不能定义具体的对象实例(比如动物), 可在基类中规定好纯虚函数接口,给子类分别去实现.

```C++
vitual int fun(member_a, menber_b)const = 0{}
```

### 多态

>C++多态(**polymorphism**)是通过虚函数来实现的，虚函数允许子类重新定义成员函数，而子类重新定义父类的做法称为覆盖(**override**)，或者称为重写。
>
>最常见的用法就是声明基类的指针，利用该指针指向任意一个子类对象，调用相应的虚函数，**动态绑定**。由于编写代码的时候并不能确定被调用的是基类的函数还是哪个派生类的函数，所以被成为“虚”函数。*如果没有使用虚函数的话，即没有利用C++多态性，则利用基类指针调用相应的函数的时候，将总被限制在基类函数本身，而无法调用到子类中被重写过的函数。*

```C++
class BaseClass
{
  virtual void funcA();
  virtual void funcB() const;
  virtual void funcC(int = 0);
  void funcD();
};
 
class DerivedClass: public BaseClass
{
  virtual void funcA() override; // ok
 
  virtual void funcB() override; // compiler error: DerivedClass::funcB() does not 
                  // override BaseClass::funcB() const
 
  virtual void funcC( double = 0.0 ) override; // compiler error: 
                         // DerivedClass::funcC(double) does not 
                         // override BaseClass::funcC(int)
 
  void funcD() override; // compiler error: DerivedClass::funcD() does not 
              // override the non-virtual BaseClass::funcD()
};
```

**finanl**表示后续派生类的虚函数不能再覆盖了.