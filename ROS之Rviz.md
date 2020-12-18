---
title: ROS之Rviz入门
date: 2020-11-12 14:02:41
categories:
- 工具技能
- ROS
tags:
- ROS
---

https://cloud.tencent.com/developer/article/1386902

我主要会向rviz发布点线等可视化信息，因此会用到**visualization_msgs::Marker** 下的**POINTS/LINE_STRIP/LINE_LIST**

参考http://wiki.ros.org/rviz/Tutorials/Markers%3A%20Points%20and%20Lines#Using_Points.2C_Line_Strips.2C_and_Line_Lists

## points

```C++
ros::NodeHandle n;
ros::Publisher marker_pub = n.advertise<visulization_msgs::Marker>("my_points_topic", 10);
ros::Rate r(30);

while(ros::ok()){
    visualization_msg::Marker points;
    
    // set header
    points.header.frame_id = "my_frame";
    points.header.stamp = ros::Time::now();
    points.ns = "my_namespace";
    points.action = visulization_msgs::Marker::ADD;
    points.pose.orientation.w = 1.0;
    
    //set other attributes
    points.id = 0;
   	points.type = visulization_msgs::Marker::POINTS;	///////////////////////////////////////
    
    //set the size of point
    points.scale.x = 0.2;
    points.scale.y = 0.2;
    
    //set corlor
    points.color.f = 1.0;
    points.color.a = 1;
    
    // add vertex coordinate
    geometry_msgs::Point p	/////////////////////////////////////////////////
     p.x = 0; p.y = 0; p.z = 0;
    points.push_back(p);
    
    marker_pub.publish(points);
    r.sleep();
}
```

## line_strip line_list

strip是按点的顺序一次连接保存多段线，[0]连[1], [1]连[2];

list中的线不是相连的,[0]连[1], [2]连[3].

```C++
visualization_msgs::Marker line_strip,line_list;
while(ros::ok()){
    //set header
    
    //set id etc.
    line_strip.type = visualization_msgs::Marker::LINE_STRIP;
    line_list.type =  visualization_msgs::Marker::LINE_LIST;
   
    line_strip.scale.x = 0.1;  	line_list.scale.x = 0.1;
    
    line_strip.color.b = 1.0;    line_strip.color.a = 1.0;
    line_list.color.r = 1.0;     line_list.color.a = 1.0;
    
    line_strip.points.push_back(p);
    // The line list needs two points for each line
      line_list.points.push_back(p);
      p.z += 1.0;
      line_list.points.push_back(p);
    
    marker_pub.publish(line_strip);
    marker_pub.publish(line_list);
}
```

