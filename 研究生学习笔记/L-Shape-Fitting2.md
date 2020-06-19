---
title: L-Shape_Fitting2
date: 2019-12-13 10:04:20
categories:
- 研究生学习笔记
- 点云处理
tags:
- L形状拟合
- 点云
- 读书笔记
---

*Come from 《Efficient Rectangle Fitting of Sparse Laser Data for Robust On-Road Object Detection 2018，IV》,也是上篇论文的衍生文章。*
# 应用文献
> F. U. Siddiqui, S. W. Teng, G. Lu, and M. Awrangjeb, “An improved building detection in complex sites using the lidar height variation and point density,” in International Conferenceon mage and Vision Computing New Zealand, 2013.

generates a height map by using height threshold and extracts only ***parallel edges*** to fit a rectangle model.

# 问题提出
最新的研究方法（上一篇文献）在某种场景下deficient失效，作者针对这一场景进行优化，并拓展到凹形点云的拟合；此外并提出“更好”的一种判据。

# 主要贡（chui）献（bi)
1. 优化了对凹形点云的拟合：
首先判断点云簇是不是凹的Concavity determination；利用k-Means方法把簇分割为几个簇分别拟合。从而拟合框占据空闲区域面积边缩小了。
![](concave.png)<center>concavity determination</center>
2. 提出一个新的拟合结果评估判据：
利用人工势场函数拟合距离d的分布直方图经验曲线（1360 clusters， 手动标记真值），自变量d是点到最近边的距离，d越小目标函数越大。计所有点的距离的函数值的和作为最终目标值函数。
作者认为真实点云是分布于轮廓附近而非BB，这也造成了 ***最大贴进度*** 判据的偏差。因此作者拟合的是轮廓框。

![](curve.png)
![](f.png)
![](优化.png)
![](CTAGC.png)<center>该判据中输入为旋转过的点云*p'*,轮廓框角点*B*; 输出判据值*Criterion*</center>

# 结果
1. NUA（normalized unoverlapped area）：未重叠的面积与真值的比
2. 角度误差
3. 计算时间

![](result1.png)
![](result2.png)
![](result3.png)
**总的来看误差分析优势不大，完全可能是有意为之挤牙膏得到的。最大的贡献应该就是解决了多一种工况。此外对于精度的讨论也局限在角度，而没有位置误差的分析。但是经验曲线和针对特定工况问题发论文的角度值得借鉴**

