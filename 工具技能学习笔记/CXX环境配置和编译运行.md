---
title: CXX环境值和编译运行
date: 2020-07-08 20:57:06
categories:
- 工具技能学习笔记
- C++
tags:
- C++
---

# 环境配置

# 编译运行

## g++和cmake,make

任何一个文本程序生成可执行文件的步骤都是:

1. 编辑器编写源代码,.cpp
2. 编译器编译代码生成目标文件,.o文件
3. 链接器链接各个目标文件,生成可执行文件,.exe

流程如下:

> 源文件-->CmakeLists-->cmake-->makefiles-->make-->.exe可执行文件.

其中gcc和g++在make阶段编译和链接文件,g++在gcc的基础上默认关联了C++库。

## cmake一个实例

https://www.cnblogs.com/haijian/p/12039160.html

<img src="/home/lee/图片/2020-07-08 20-59-28屏幕截图.png" style="zoom:50%;" />