---
title: PCL入门(一)
date: 2019-12-17,11:00:00
categories: 
- 研究生
- PCL
tags:
- PCL
---

PCL在windows中配置环境十分麻烦，相反在ros中非常方便。
**学习资料**:
[http://pointclouds.org/documentation/tutorials/](http://pointclouds.org/documentation/tutorials/)

[工作空间配置实例](https://blog.csdn.net/qq_42367689/article/details/104358046)

## [PCL：点云数据格式bin转pcd](https://blog.csdn.net/qq_40297851/article/details/85274563#commentBox)
注意cMakeList.txt中变量名和路径的统一. 在ROS中表示点云的数据结构有 pcl::PointCloud < T >, 而msg中常用pcl::PointCloud2. 他们之间的转换使用命令`pcl::fromROSMsg` 和 `pcl::toROSMsg`

```C++
pcl::PCDReader reader;
reader.read<pcl::PointXYZ> ("/home/lijie/bin2pcd_ws/src/sor_filter/src/table_scene_lms400.pcd", *cloud);
pcl::io::loadPCDFile ("/home/lijie/catkin_ws/src/pcd_load/13.pcd", cloud);
```

## [**PCL：PCD文件拼接**](https://blog.csdn.net/ethan_guo/article/details/80110023)

## [**PCL：下采样和地面过滤**](https://blog.csdn.net/AdamShan/article/details/82901295)

## [数据集的读取和滤波处理](https://www.cnblogs.com/li-yao7758258/p/6651326.html)

**Q&A:**  

1. 在一个package/src中建立两个*.cpp节点,分别实现数据的读取和发布、数据的预处理两个功能。
需要修改CMakeLists.txt文件。projectName是package的名字而不是节点名，将原本生成可执行文件命令和链接目标命令中的`${PROJECT_NAME}_node`（即节点名）修改为你的节点名（建议与*.cpp一致或添加*_node）如下：

```
 ## Declare a C++ executable
 ## With catkin_make all packages are built within a single CMake context
 ## The recommended prefix ensures that target names across packages don't collide
add_executable(**pcd_load_node**  src/pcd_load.cpp )
target_link_libraries(pcd_load_node ${catkin_LIBRARIES} )

add_executable(pcd_pub_node  src/pcd_pub.cpp )
target_link_libraries(pcd_pub_node ${catkin_LIBRARIES} )
```

2. 在建立数据读取和发布节点**pcd_pub.cpp**时：
   注意点云数据格式转换：`pcl::toROSMsg(pcl::PointXYZI, sensor_msgs::PointCloud2)`, `pcl::formROSMsg( )`,他们包含于				`pcl_conversions/pcl_conversions.h`头文件中。发布到topic中时若需要在rviz中显示，这需要fix_frame命令：

```
topic.header.frame_id="velodyne";//是后面rviz的 fixed_frame
```

3. 在建立数据预处理节点**pcd_load.cpp**时：
需要注意的依然是数据格式问题：订阅器查询时会调用回调函数（filter），将topic中的msg传递过去，所以输入是PointCloud2类型引用。因为各个滤波器的输入是指针而非点云数据，所以应该转换为指针：
	
	```
	pcl::PointCloud<pcl::PointXYZI>::Ptr scan_ptr(new pcl::PointCloud<pcl::PointXYZI>);
	pcl::fromROSMsg(*cloud, *scan_ptr);
	```
其中`new pcl::PointCloud<pcl::PointXYZI>（scan）`用于初始化指针指向scan类所在地址，也可不申明指向对象。
	
4. 使用直通滤波器时，要分别进行x，y，z方向的输入设置，依然为指针。
```
  pcl::PassThrough<pcl::PointXYZI> pass;
  pass.setInputCloud(pcd_filtered_ptr);
  pass.setFilterFieldName("x");
  pass.setFilterLimits(-10.0,10.0);
  pass.filter(*pcd_filtered_ptr);

  pass.setInputCloud(pcd_filtered_ptr);
  pass.setFilterFieldName("y");
  pass.setFilterLimits(-5.0,5.0); 
  pass.filter(*pcd_filtered_ptr); 
  std::cerr << "Cloud after RoIfiltering: " << std::endl;
  std::cerr << *pcd_filtered_ptr << std::endl;
```

## 地面分割：
Ray Ground Filter的路面过滤方法。
[SAC_RANSAC分割地面](https://blog.csdn.net/HHH_go_/article/details/83148472)

## PCL/cluster
 找问题真的很费时间，一些没遇见过的小错误就很难发现。记录一下。
 ### 1. 欧式聚类实操
 体素网格下采样尺寸太小，数据量太大，Integer indices would overflow. 指针溢出。
 但若网格尺寸太大，聚类的`ec.setClusterTolerance (0.01)`公差比它小则聚类数量为0。
 ### 2.投影到平面
 [点击查看教程](http://pointclouds.org/documentation/tutorials/project_inliers.php#project-inliers)
 由于通常是投影到xy平面可以使用循环代码：cloud_cluster是点云的指针. 
 ````
 for(int i = 0; i < cloud_cluster->points.size(); ++i)
      cloud_cluster->points[i].z=0; 
 ````
### 3.提取边界
Q1:
````
[pcl::BoundaryEstimation::initCompute] The number of points in the input dataset (23798) differs from the number of points in the dataset containing the normals (884)!
````





```
// 地面分割代码块
#include<ros/ros.h>
#include<pcl/point_cloud.h>
#include<pcl_conversions/pcl_conversions.h>
#include<sensor_msgs/PointCloud2.h>
#include<pcl/io/pcd_io.h>

int  main(int argc,char **argv)
{
  ros::init (argc, argv, "pcd_pub_node"); 
  ros::NodeHandle nh;

  pcl::PointCloud<pcl::PointXYZI> cloud;
  sensor_msgs::PointCloud2 input;
 
  pcl::io::loadPCDFile ("/home/lijie/catkin_ws/src/pcd_load/13.pcd", cloud); 
  pcl::toROSMsg(cloud,input);

  input.header.frame_id="velodyne";   //是后面rviz的 fixed_frame
  ros::Publisher pcl_pub = nh.advertise<sensor_msgs::PointCloud2> ("pcd_input", 10);
  
  ros::Rate r(1);
  while(ros::ok())
  {
	  pcl_pub.publish(input);
	  ros::spinOnce();
	  r.sleep();
  }
  return 0;
 }

```

```
#include<ros/ros.h>
#include<pcl/point_cloud.h>
#include<pcl_conversions/pcl_conversions.h>
#include<sensor_msgs/PointCloud2.h>
#include<pcl/io/pcd_io.h>//which contains the required definitions to load and store point clouds to PCD and other file formats.
#include<pcl/filters/statistical_outlier_removal.h>
#include <pcl/point_types.h>
#include<pcl/filters/voxel_grid.h>

ros::Publisher pcl_pub;

void filter (const sensor_msgs::PointCloud2ConstPtr& cloud)
{
  // pcl::PointCloud<pcl::PointXYZ> scan;
  pcl::PointCloud<pcl::PointXYZI>::Ptr scan_ptr(new pcl::PointCloud<pcl::PointXYZI>);
  pcl::fromROSMsg(*cloud, *scan_ptr);
  std::cerr << "Cloud before filtering: " << std::endl;
  std::cerr << *scan_ptr<< std::endl;

  pcl::VoxelGrid<pcl::PointXYZI> vg;//体素滤波
  vg.setLeafSize (0.1,0.1,0.1);
  vg.setInputCloud(scan_ptr);   //输入为指针!!!!
  
  // pcl::PointCloud<pcl::PointXYZ> pcd_filtered;
  pcl::PointCloud<pcl::PointXYZI>::Ptr pcd_filtered_ptr(new pcl::PointCloud<pcl::PointXYZI>);
  vg.filter (*pcd_filtered_ptr);
  std::cerr << "Cloud after filtering: " << std::endl;
  std::cerr << *pcd_filtered_ptr << std::endl;

  pcl::StatisticalOutlierRemoval<pcl::PointXYZI> sor; //Kmeans滤波,参数临近点数目和距离阈值
  sor.setInputCloud(pcd_filtered_ptr);
  sor.setMeanK (20);
  sor.setStddevMulThresh(1.0);
  pcl::PointCloud<pcl::PointXYZI> pcd_filtereded;
  sor.filter (*pcd_filtered_ptr);

  sensor_msgs::PointCloud2 filter_output;
  pcl::toROSMsg(*pcd_filtered_ptr, filter_output);
  pcl_pub.publish (filter_output);
}


int main(int argc,char** argv)
{
  ros::init (argc, argv, "pcd_fileter_node"); //初始化
  ros::NodeHandle nh;
  ros::Subscriber sub = nh.subscribe ("pcd_input", 5, filter);
  pcl_pub = nh.advertise<sensor_msgs::PointCloud2> ("pcl_fileter_output", 5);
  
  ros::spin();
  return 0;
}
```


