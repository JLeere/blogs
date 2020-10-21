---
title: 泊车感知框架阅读笔记（一）
date: 2020-06-28 14:22:12
categories:
- 研究生
tags:
- lidar
---

# lidar_perception 调试

## Github

从Chen的仓库中克隆最新分支dev,并转移到自己新建的分支dev_Lee

```shell
git clone http://***
git checkout dev
git checkout -b dev_Lee
```

## 文件架构

**根目录**:拉取下来的lidar_perception是ros工作空间的一个packege
**一级目录**:

- include(头文件.h,每个阶段步骤所包括的类及其方法的声明),
- src(源文件.cpp,对头文件类的方法的具体实现进行描述,类外申明),
- node(*.cpp,调用其他各类方法的主函数,还包括与ros进行通信),
- rviz(显示的配置设置)

## catkin_make

配置好packege.xml和CMakeLists,成功catkin_make编译后,会在workspace的devel文件夹下生成release版本的可执行文件.

>cmake和catkin_make编译都是生成release版本,优化较好跑得快,而调试用生成debug版本,可以设置断点。

打开roscore,运行主节点node,将采集的点云包rosbag文件play,自动发布到topic中,打开rviz进行显示

```
roscore
rosrun <rospackage> node
rosbag play ~/rosbagfiles/**.bag -l -r 0.1
rviz
```

当然，更方便的方法就是将多节点运行顺序写到roslaunch文件中，一键启动。
`roslaunch <rospackage> **.launch`

## 算法细节笔记

- 基于环视图方法组织点云, 进行地面切除和点云聚类.
- 再在鸟瞰图中拟合, 限定拟合框的长宽比, 筛选出车辆类型的拟合框.

<!-- more-->

# 自定义接口

## my_point_type.h

```C++
struct PointXYZIR
{
    PCL_ADD_POINT4D //???
    float intensity;
    uint16_t ring
    EIGEN_MAKE_ALIGNED_OPERATOR_NEW // make sure our new allocators are aligned
    PointXYZIR(){}
    PointXYZIR(float x, float y, float z, float intensity, uint16_t ring):x(x),y(y),z(z),intensity(intensity),ring(ring){}
} EIGEN_ALIGN16;

using RichPoint = PointXYZIR;
using CloudType = pcl::PointCloud<RichPoint>;
using VectorType = typename pcl::PointCloud<RichPoint>::VectorType;
using CloudTypePtr = typename CloudType::Ptr;
using CloudTypeConstPtr = typename CloudType::ConstPtr;
using CloudTypePtrList = std::vector<CloudTypePtr>;
```

## 定义CTimer

在聚类中,的**目的**是干嘛?

# todo

- ~~共享指针make_shared~~
- ~~Eigen库~~

# to ask

```C++
RangeMapCloudPtr cloud = boost::make_shared<RangeMapCloud>(std::move(*pcl_cloud));
//智能指针

assert(nullptr == _projection);

RangeMapCloud(RangeMapCloud &&cloud) : _projection{std::move(cloud._projection)},//初始化列表
_tensor_class{cloud._tensor_class}
// _sensor_pose{cloud._sensor_pose}
{
    ((Base *)this)->swap(cloud);
}

virtual double _calc_criterion(const Eigen::MatrixX2f &points_mat, const double angle_rad, BBox &box) const override;
//虚拟函数,可让派生类自己写定义
//override,覆盖父类的同名函数定义

auto l_fitting = LOrientationFitting();//后者是一个类,这么初始化怎么回事?
// A: 赋值构造函数.
```

# 库函数调用记录

## openCV

## PCL









































































































































































