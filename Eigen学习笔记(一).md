---
title: Eigen学习笔记
date: 2020-10-13 20:05:29
categories:
- 编程学习
tags:
- Eigen
---

参考网站: https://zhuanlan.zhihu.com/p/36772345

[参考博客](http://zhaoxuhui.top/blog/2019/08/21/eigen-note-1.html)

官方教程: https://eigen.tuxfamily.org/dox/group__QuickRefPage.html

# CMake库中的声明

Eigen库本身全是头文件，只需要将头文件包含到路径中即可。

## 方法1 修改vscode环境配置

[vscode配置C++程序.md](./vscode配置C++程序.md)

## 方法2 CMake工具

利用Cmake文件将包含路径写进去。

*cmake也可以在vscode中利用cmake tool工具读入，自动包含路径，而无需再对vscode进行配置*

```CMAKE
#寻找库Eigen
find_package(Eigen3 REQUIRED)
#把库包含进去
include_directories(${EIGEN3_INCLUDE_DIRS})
#因为只有头文件，因此不需要target_link_libraries
```



# 矩阵和向量

## 初始定义

```C++
#include<iostream>
#include <Eigen/Dense>

using namespace Eigen;
using namespace std;

int main(){
    //started
    MatrixXd m(2,2);
    m(0,0) = 3;
    m(1,0) = 2.5;
    m(0,1) = -1;
    m(1,1) = m(1,0) + m(0,1);
    cout << m << endl;
	
    //动态大小的矩阵
    MatrixXd M1 = MatrixXd::Random(3,3);
    M1 = (M1 + MatrixXd::Constant(3,3,1.2));
    cout<<"M1= "<<M1<<endl;

    VectorXd v(3);
    v <<1,2,3;
    cout<<"v*M1 = "<<endl<<M1*v<<endl;
	
    //固定的矩阵
    Vector3d v2(1,2,3);
    Matrix3d M2 = Matrix3d::Random();
    M2 = M2 + Matrix3d::Constant(1.2);
    cout<<"M2*v2= "<<endl<<M2*v2<<endl;
    Matrix3d M3;
    //逗号初始化
    M3 << 1,2,3,
        	1,4,6,
    		2,3,5;		
    
    //矩阵变换
    MatrixXcf a = MatrixXcf::Random(2,2);//定义2-by-2随机矩阵
    cout << "Here is the matrix a\n" << a << endl;//矩阵a
    cout << "Here is the matrix a^T\n" << a.transpose() << endl;//a的转置
    cout << "Here is the matrix a^H\n" << a.conjugate() << endl;//a的共轭
    cout << "Here is the matrix a^{-1}\n" << a.inverse() << endl;//a的逆

    //向量乘法
    Vector3d v(1,2,3);
    Vector3d w(0,1,2);
    cout << "Dot product: " << v.dot(w) << endl;//向量点乘
    cout << "Cross product:\n" << v.cross(w) << endl;//向量叉乘
}
```

## 其他函数

## 获取参数

`.col(),.row(),.size()`：获取列数行数和元素个数

`.data()`: 返回矩阵首地址的指针

```C++
MatrixXd::Zeros(n,m);
MatrixXd::Ones(n,m);
MatrixXd::Identity(n,m);
MatrixXd::LineSpaced(size,low,high)
```

