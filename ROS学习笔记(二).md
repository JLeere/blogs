---
title: ROS学习笔记(二)
date: 2020-10-10 11:00:00
categories: 
- 工具技能
- ROS
tags:
- ROS
---

# ROS接口编写

```c++
#include <ros/ros.h>

int main(int argc,char** argv){
    ros::init(argc,argv,"listener");
    ros::NodeHandle nh;
    ros::Subscriber subscriber = 
        nh.subscribe("chatter",10,callback);
    //ros::Publisher publiser = 
    //  nh.advertise<std_msgs::String>("chatter",1);
    // publisher.publish(msg1);
    ros::spin();
    return 0;
}
```

<!-- more-->

