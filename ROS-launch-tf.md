---
title: ROS-launch启动文件 & tf
date: 2020-10-05 20:20:25
categories:
- 工具技能
- ROS
tags:
- ROS
- launch
---

# launch

可以启动多个节点, 并传入节点需要的参数变量等. 同时也不需要先启动roscore了. 

## 简单实例

```xml
<launch>
	<node pkg = "lidar_perception" type = "slot_detection" name = "detecting slots" />
    
    <param name = "slot_width" value="1.7" />
    <arg name="thershold_length" value="1.5" />
    <node pkg="lidar_perception" type="slot_detection" arg="${arg threshold_length}" name "detection slots" />
</launch>
```

**标签node**表示运行包pkg, 中的可执行cpp文件type (若使用py脚本需要添加.py后缀), 然后将这个节点的ROS名字取为name. 还可以添加需要传入的变量参数,用arg传入. 

**标签param**表示定义一些参数名及其值. **rosparam**可以传入参数文件中所有参数.

**标签arg**定义launch内部使用的变量参数,可供其他使用.

```xml
<launch>
	<remap from="slot_detection/thresh_l" to="thresh_l" />
    <include file="${dirname}/other.launch" />
</launch>
```

**标签remap**重新定义topic(或其他?)所有资源的名字, 

**标签include**嵌套使用另外一个launch文件.

# tf坐标系

