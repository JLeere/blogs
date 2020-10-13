---
title: Matlab小知识记录
date: 2020-03-23 13:24:30
categories:
- 编程学习
- Matlab
tags:
- 小知识
---

当然详细的内容参照官网文档，这里记录一些常用的或通常误解的。

# 固定步长和变步长
1. 一般情况下，减小步长大小将提高结果的准确性，但会增加系统仿真所需的时间。
2. 可变步长求解器会改变仿真期间的步长大小：当模型状态快速变化时，减小步长大小以提高准确性；当模型状态缓慢变化时，增加步长大小以避免执行不必要的时间步。计算步长大小会增加每个步长的计算开销，但可以减少对具有*快速变化的状态或分段连续状态的模型*维护指定级别的准确性所需的总时间步数，从而缩短仿真时间。

# Matlab funtction模块
1. **%#codegen**：在function 头的下一行增加%#codegen符号，其作用是为了使静态代码分析器Code Analyzer 诊断代码并提示用户对可能在代码生成的过程中导致错误的违规写法进行修正。
此模块自带此功能，不用特地声明
2. 内部语言因为要编译成C，因此MATLAB Function内部的M语言**变量**必须要给定初始值及其维度，变量类型及其虚实性，不支持变维度变量
3. 对于每次调用该函数块时需要循环使用的变量，可申明为**persistent**变量。注意：与global不同的是其只能在函数内部被识别，申明时为空，需要初始化赋值。
4. 其可以调用大部分工具箱的函数，支持的函数列表见```>>doc C/C++ 代码生成支持的函数和对象 - 按类别排列 ```，但是Matlab Function输出不支持高级别的Class，比如pointCloud类，会报错：
>A top-level output parameter containing a class is not supported in MATLAB Function Block. Output parameter 'pointCd' contains a class.
5. **Ports and Data Manager**:
输出变量长度如果不同时刻会变化，应将其Size属性设置为Variable size，并在前面写下size的上限值。
![](1.png)
6. 也不能把某个工具箱的类变量作为输出，C语言不符合
7. C默认float是single精度，而m语言默认是double类型，精度高但是占内存。可以用`a=single(a)`转换，或者Type Conversion模块转换数据格式。

# prescan仿真问题记录
1. 构造pointCloud函数一旦放到Matlab Function中就报错，说输入的矩阵不是M by 3的形式，但事实就是M*3。断开prescan单独拎出去就可以，真是奇了怪了？干脆不用point Cloud工具箱了，直接写聚类这些。



