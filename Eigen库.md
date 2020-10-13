---
title: Eigen库
date: 2020-10-12 17:05:29
categories:
- 编程学习
- C++
tags:
-Eigen
---



参考网站: https://zhuanlan.zhihu.com/p/36772345

官方教程: https://eigen.tuxfamily.org/dox/group__QuickRefPage.html

[vscode配置C++程序.md](vscode配置C++程序.md)

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

    MatrixXd M1 = MatrixXd::Random(3,3);
    M1 = (M1 + MatrixXd::Constant(3,3,1.2));
    cout<<"M1= "<<M1<<endl;

    VectorXd v(3);
    v <<1,2,3;
    cout<<"v*M1 = "<<endl<<M1*v<<endl;

    Vector3d v2(1,2,3);
    Matrix3d M2 = Matrix3d::Random();
    M2 = M2 + Matrix3d::Constant(1.2);
    cout<<"M2*v2= "<<endl<<M2*v2<<endl;

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

