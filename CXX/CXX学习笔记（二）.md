---
title: CXX学习笔记（二）
date: 2020-07-08 17:57:06
categories:
- 编程学习
- C++
tags:
- C++
---

对于书籍《数据结构与算法分析——C++语言描述第四版》的学习笔记

# 指针pointor和引用&

## 指针

`IntCell *m`是关于m的申明，m是指针变量，指向一个IntCell对象，m的值是指向对象的地址，此时尚未初始化。

初始化（动态创建）：

`m = new Intcell();`

`m = new Intcell{};//C++11`

`m = new Intcell;`

当通过new操作符分配地址的对象不再被引用时候，必须进行delete进行垃圾回收，否则指针所占用的内存将会一直被丢失得不到利用知道程序终止。

`delete m`

## & 取地址操作符，引用

### 左值引用

- 给复杂的名称取别名

  `auto & List = theLists[myhash(x,theLists.size())]`

  这样对List进行操作就是对原对象进行操作，若不加引用则是对拷贝对象进行操作，原内容并无变化。

- 范围for循环

  ```C++
  //让arr数组中每个变量加1
  for(auto X:arr)
      ++X;		//不可行,x拷贝了每一个元素
  for(auto & x:arr)
      ==x;		//可行
  ```

- 避免不必要的拷贝

  ```C++
  auto x = findMax(arr);
  auto &x = findMax(arr);//没有对数组中的最大值进行拷贝
  ```

### 右值引用

```C++
const int x = 2;
int y;
string str = "hello";//x,y,str都是左值,2,hello是右值
string & bad = "hello";//错误,此乃左值引用,"hello"为不可修改的右值
string && good = "hello";//合法
```

# 参数传递

## 输入

对于输入到函数的参数对象：

> 对于小的不该被函数改变的对象,可以采取**传值调用.**
>
> 对于大的不该被函数改变的复制代价昂贵的对象,应采取**传常量引用调用**
>
> 对于所有可以被函数改变的对象,应该采取**传引用调用**.

常用的传值调用将实参复制到形参,对于大的对象效率低,且不能改变实参。而采用传引用调用就可以在函数内部改变传入的实参，且不会复制代价。若输入参数不希望改变且较大，这使用传常量引用调用。

```C++
string randomItem(const vector<string> & arr);
```

## 返回

传值返回。

传常量引用

```C++
const Largetype & randomItem2（const vector<Largetype> & arr)
{...}
Largetype a = randomItem(vec);//返回值发生了复制
Largetype b = randomItem2(vec);//复制
Largetype & c =randomItem2(vec);//没有复制
```

# 构造函数

初始化表列：

```C++
：membervalue{initalValue}{}//比如数据成员为const型，因此只能在初始化表列中初始化。
```

explicit构造函数，英文原意“明确的不含糊的”，为了申明隐式的类型转换是不可行的。

```C++
public:
	explicit IntCell(int initialValue = 0)
    	: membervalue{initalValue}{}
	int read() const	//常成员函数，访问函数，表示不改变对象数据成员
    {return membervalue;}
	void write(int x)	//修改函数，可修改数据成员，但是不能改变常对象。
    {membervalue = x;}
private:
	int membervalue;
```

举例explicit的作用：

```C++
IntCell N;
N = 20; 	//类型不匹配，但是C++隐式类型转换会先创建临时IntCell对象，再赋值给N。
//而添加explicit后就会指出这个问题
```

当数据成员为指针类型时，默认的几类构造函数将不起作用，他们只是对指针地址进行了**浅拷贝**,而我们需要的是对指向的内容进行**深拷贝**,因此需要自己写:

- 析构函数

- 复制构造函数

- 移动构造函数

- 拷贝赋值

  ```C++
  IntCell & operator = (const IntCell & rhs)
  {
      if(this != &rhs)
          *membervalue  = *rhs.membervalue;
      return *this;
  }
  ```

  

- 移动复制

