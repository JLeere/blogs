---
title: Eigen学习笔记(三)
date: 2020-10-28 10:18:20
categories:
- 编程学习
tags:
- Eigen
description: `#include <Eigen/Gometry>
---

x学习链接: http://zhaoxuhui.top/blog/2019/08/21/eigen-note-1.html

# 旋转

## 2D旋转

**Rotation2D**类, 构造参数可以输入旋转角度, 2*2旋转矩阵, 另一个Rotation2D类. 注意顺时针旋转为正,逆时针为负值.

成员函数:

- `.cast()`, 可以将Rotation2Df对象与Rotation2Dd对象类型相互转换
- `.angle()`, 返回旋转角
- `.matrix()`, 返回旋转矩阵
- `.toRotationMatrix()`, 将当前Rotation2D对象转变为2*2矩阵
- `.fromRotationMatrix()`, 输入矩阵, 构建Rotation2D对象.
- `.inverse()`, 相反方向旋转