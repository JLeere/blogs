---
title: PCL基础知识
date: 2020-10-10 11:00:00
categories: 
- 研究生
- PCL
tags:
- PCL
---

[官方文档](https://pcl.readthedocs.io/projects/tutorials/en/latest/walkthrough.html#common)

## PCL点云数据结构

[学习链接](https://blog.csdn.net/qq_30815237/article/details/86475877?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)

### **PointCLoud**:

+ width(int),每一束激光扫描的点数
+ height(int), 激光的线数, 当点云为无序点云时候height=1
+ points(std::vector), 存储点的类型的向量,如XYZ,XYZI等,

<!-- more-->

```C++
pcl::PointCloud<pcl::PointXYZ> cloud;
cloud.points[i].x = 1;
std::vector<pcl::PointXYZ> data = cloud.points;
if(!cloud.isOrganized()){}//判断是不是有序点云
is_dense(true)//指定所有点都是稠密的,inf/nan
    
//指针类型
pcl::PointCloud<pcl::PointXYZ>::ptr cloud2(new pcl::PointCloud<pcl::PointXYZ>);
cloud2->point[i].x = 1;
```

1. PointXYZ

   结构: `float x,y,z ` 

   用cloud.points[i].x访问

2. PointXYZI

   结构:`float x,y,z,Indensity;`

   用cloud.points[i].Data[4]访问强度

3. PointXYZRGB

   结构:`float x,y,z,rgb` rgb用一个浮点数表示

