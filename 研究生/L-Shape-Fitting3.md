---
title: L-Shape-Fitting3
date: 2019-12-16 10:37:20
categories:
- 研究生
- LiDAR
tags:
- L形状拟合
- 点云
- 读书笔记
---
From 《LiDAR-Based Object Tracking and Shape Estimation Using Polylines and Free-Space Information》
[链接](https://ieeexplore.ieee.org/abstract/document/8593385#full-text-header)
# 主要贡献
1. 将**位姿信息估计** 、**多段线拟合的形状估计** 二者结合同时推导跟踪车辆
2. 对激光束点和传感器之间的 **自由空间** 利用起来，改进跟踪器。
![](L-Shape-Fitting3/1.gif)

<!-- more -->

# 算法流程
## 状态跟踪(X)和形状估计(c)相互优化
![](L-Shape-Fitting3/2.gif) ![](L-Shape-Fitting3/3.png) ![](L-Shape-Fitting3/4.png) ![](L-Shape-Fitting3/5.png)
## laser scan  free-space 未占据的区域检测
![](L-Shape-Fitting3/6.png) ![](L-Shape-Fitting3/7.png) ![](L-Shape-Fitting3/8.png)
# 主要结果

# 思考
方法看得不是很明白,他的重心不在怎么拟合,而是通过拟合形状与位姿信息最优化出跟踪信息.对自由空间的利用有空可以看看对泊车有什么启发
