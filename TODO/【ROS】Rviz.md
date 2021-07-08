---
title：【ROS】Rviz基础几何绘制
date: 2021-01-14 10:36:17
categories:
- 编程学习
tags: 
- ROS
---

Rviz教程：http://wiki.ros.org/rviz/Tutorials

```C++
//线条
visualization_msgs::Marker OV_line,PS_line;
while(ros::ok()){
    //set header
    OV_line.header.frame_id = PS_line.header.frame_id =  header_parkingSlot.frame_id;
    OV_line.header.stamp = PS_line.header.stamp = header_parkingSlot.stamp;
    OV_line.ns = PS_line.ns = "rslidar_perception";
    OV_line.action = PS_line.action = visualization_msgs::Marker::ADD;
    OV_line.pose.orientation.w = 1.0;

    OV_line.id = 0;
    PS_line.id = 1;
    OV_line.type = PS_line.type = visualization_msgs::Marker::LINE_STRIP;

    OV_line.scale.x=0.1;
    PS_line.scale.x = 0.1;
    OV_line.color.b = 1.0;
    PS_line.color.r = 1.0;

    OV_line.points.push_back()
        geometry_msgs::Point P_PS,P_OV;
    P_PS.x = PS_pose_filtered[0]+1.5*cos(PS_pose_filtered[2]);
    P_PS.y = PS_pose_filtered[1]+1.5*sin(PS_pose_filtered[2]);
    P_PS.z = 0;
    PS_line.points.push_back(P_PS);
    P_PS.x = PS_pose_filtered[0];
    P_PS.y = PS_pose_filtered[1];
    PS_line.points.push_back(P_PS);
    P_PS.x = PS_pose_filtered[0]+2.5*cos(PS_pose_filtered[2]-M_PI_2);
    P_PS.y = PS_pose_filtered[1]+2.5*sin(PS_pose_filtered[2]-M_PI_2);
    PS_line.points.push_back(P_PS);

    P_OV.x = OV_pose_filtered[0]+1.5*cos(OV_pose_filtered[2]);
    P_OV.y = OV_pose_filtered[1]+1.5*sin(OV_pose_filtered[2]);
    P_OV.z = 0;
    OV_line.points.push_back(P_OV);
    P_OV.x = OV_pose_filtered[0];
    P_OV.y = OV_pose_filtered[1];
    OV_line.points.push_back(P_OV);
    P_OV.x = OV_pose_filtered[0]+2.5*cos(OV_pose_filtered[2]-M_PI_2);
    P_OV.y = OV_pose_filtered[1]+2.5*sin(OV_pose_filtered[2]-M_PI_2);
    OV_line.points.push_back(P_OV);

    tracked_slot_marker_pub.publish(PS_line);
    marker_pub.publish(OV_line);
}
    
// 用箭头来代替有向点

```

