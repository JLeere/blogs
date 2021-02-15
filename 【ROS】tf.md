---
title: 【ROS】transform变换矩阵
date: 2020-12-29 21:39:44
tags: 
- ROS
---

学习视频：https://www.bilibili.com/video/BV1mJ411R7Ni?p=31

*之前零零碎碎了解了很多，但是都没系统性的学习和实现，在这里详细的总结一下每个知识，加强记忆*

## **描述规范：**

1. source、target frame是在**进行坐标变换**时的概念，source是坐标变换的源坐标系，target是目标坐标系。这个时候，这个变换代表的是**坐标变换**。
2. parent、child frame是在**描述坐标系变换**时的概念，parent是原坐标系，child是变换后的坐标系，这个时候这个变换**描述的是坐标系变换**，也是child坐标系在parent坐标系下的描述。
3. a frame到b frame的坐标系变换（frame transform），也表示了b frame在a frame的姿态描述，也代表了把一个点在b frame里坐标变换成在a frame里坐标的**坐标变换**。
4. 从parent到child的坐标系变换（frame transform）等同于把一个点从child坐标系向parent坐标系的**坐标变换**，等于child坐标系在parent frame坐标系的姿态描述。

二者其实是等价的, 坐标系变换会反转一下,本子上依然是坐标系原点的坐标变换,但是该坐标系中的坐标依赖于它,因此会反一下,不难理解.

## **坐标变换举例**

- 我记忆，为了防止混淆，都以坐标变换为基础，而不在考虑坐标系变换的角度，也不要用旋转平移的动态视角去记忆。即统一利用描述规范中的1,4点进行记忆，编程时不易出错

- <center><img src="file:///home/jlee/文档/github_repositories/Blog/source/_posts/坐标变换/image-20201113151857377.png?lastModify=1612318184" alt="image-20201113151857377" style="zoom: 80%;" div style="align: center"  /></center>

- 现在我们要将局部坐标系O‘下的P点坐标（记为P_O' ）转换到全局坐标系O下，即获得全局坐标系O的P点坐标(记为P_O) 。按照规范1来讲，O‘是source，O是target

- 首先我们要获取变换矩阵transform，记为T_O_O'，意味着从O‘到O坐标系的坐标变换矩阵。transform矩阵的参数本质也是个pose，是局部坐标系O' 在全局坐标系O中的pose，即设置translate参数为**O‘的坐标**，rotation参数为**O‘ x正半轴的角度**（即逆时针为正）。——与之前旋转平移原坐标系到新坐标系一致。

- $P_O' =T_{OO'}*P_{O'}$ 类似与向量乘法，大部分库都是默认变换矩阵**左乘**，因此我们从右向左看。$P_{O'}$ 为**列向量**，因此 $T_{OO'}$ 是**从右往左**依次进行运算，表示**从O'到O的坐标变换**.

- $P_a =T_{aO}T_{OO'}*P_{O'}$ 上式再左乘一个矩阵，从右往左看，$T_{aO}T_{OO'}=T_{aO'}$ , 下标依次抵消，最后就可以得到P在a坐标系下的坐标。

## tf tree

