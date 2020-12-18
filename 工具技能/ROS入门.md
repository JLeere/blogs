---
title: ROS入门
date: 2019-12-17,11:00:00
categories: 
- 工具技能
- ROS
tags:
- ROS
---
官方学习文档：[http://wiki.ros.org/](http://wiki.ros.org/)

很好的学习视频：https://www.bilibili.com/video/BV1zt411G7Vn?t=104&p=21

很好的学习论坛古月居：https://www.guyuehome.com/

## ROS准备

[安装ROS](http://wiki.ros.org/kinetic/Installation/Ubuntu)
[建立ROS工作空间](https://www.cnblogs.com/huangjianxin/p/6347416.html)

```C++
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
catkin_init_workspace    //初始化生成cmake文件
cd ~/catkin_ws
catkin_make              // 编译生成可执行文件
source devel/setup.barsh // 刷新环境变量
```

**后面进行package创建时，当加入了新的package编译完成后，也要进行source刷新环境变量，否则会出现找不到“package XXX not found” 的问题**
用下面指令将其写入文件中，避免每次打开终端都需要刷新工作环境：

```C++
echo "source my_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

可以查看系统包含的package路径

```C++
echo $ROS_PACKAGE_PATH
```

## ROS概念

#### **参考资料**:

 [https://blog.csdn.net/AdamShan/article/details/79653378](https://blog.csdn.net/AdamShan/article/details/79653378)  
  >建议将 ROS 接口节点（订阅，发布）和算法结点分开。
  [MOOC ROS入门视频：](https://www.bilibili.com/video/av24585414/?p=5)

#### ROS目录结构

  node, master, topic, subscribe, publisher,msg
 <img src="https://img-blog.csdnimg.cn/20190424195935783.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_5,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTc5NDczMw==,size_5,color_FFFFFF,t_20" alt="ROS工程结构" style="zoom:50%;" />

> ROS的软件使用package（包）进行组织，包下通常包含一下内容：
`/src`: 源代码
`/msg`: 定义一些message
`/srv`: 定义一些service
`/launch`：包含用于启动节点的launch file
`/config`：包含配置文件
`/test`: Unit/ROS tests
`/include/package_name`: C++ include头文件
`/doc`：包含文档文件
`package.xml`: package 信息
`CMakeLists.txt`: CMake构建文件

## ROS项目操作

#### 基本指令

 roslaunch 启动master和多个node程序
 `roslaunch [pkg_name] [file_name.launch]`

rostopic
`rostopic list` 列出当前所有topic
 `rostopic info / topic_name` 显示某个topic属性
 `rostopic echo / topic_name`显示某个topic内容
 `rostopic pub /topic_name ...` 向某个topic发布内容

rosmsg
`rosmsg list`列出系统上所有消息msg
`rosmsg show /msg_name` 显示某个消息msg内容

`rostopic echo [topic]` 打印topic详细信息
`rqt_gragh` 查看节点图

## **编译实例**

### [Hello world](https://blog.csdn.net/AdamShan/article/details/79882668)

**流程：**

创建工作空间 `mkdir catkin_ws/src/`
创建 `catkin_create_pkg package_name depend1 depend2 depend3(pcl_ros roscpp sensor_msgs)`
创建节点node  `*.cpp`
修改**CMakeList** 和 **package.xml**
编译 `catkin_make`, `source /devel/setup.bash`
终端命令:`roscore`; `rosrun package node`; `rosrun rviz rviz;`
其他常用命令:`rosnode list`; `rostopic list`; `roscd`

**代码**:
 `ros::Rate`循环刷新频率10HZ
 `ros::ok()`节点运行结束这返回false
 `ros::spin(); ros::spinOnce()`不断查询订阅的话题，执行回调函数
 `Logging` 不推荐使用std::cout

## CmakeList

 [**ROS：依赖文件和环境**](https://blog.csdn.net/AdamShan/article/details/82901295)
 [**CMakeList详细解读**](https://www.cnblogs.com/Jessica-jie/p/6520481.html)
 视频讲解的更加基础。
 CMakeLists.txt文件是CMake构建系统的输入，在这里我们不会详细讨论CMake的写法（因为它本身可以很复杂），我们大致熟悉一下我们常用的CMake的语法：

`cmake_minimum_required：`需要的CMake的最低版本
`project():`包的名称
`find_package()` 查找建构是需要的其他 CMake/Catkin 包
`add_message_files() add_service_files() add_action_files` 生成Message/Service/Action
`generate_messages()` 调用消息生成
`catkin_package()` 指定包的构建信息
`add_library()/add_executable()/target_link_libraries()` 用于构建的库，可执行代码

同样的，在`CMakeList`中，我们通过`find_package`查找这三个包的路径，然后将三个包添加到 `CATKIN_DEPENDS`, 在使用pcl库前，需要将PCL库的路径链接，通过`link_directories( $ {PCL_LIBRARY_DIRS})`来完成，并在最后的`target_link_libraries`中添加`${PCL_LIBRARIES}`。

 `package.xml`的内容很简单，实际上就是这个包的描述文件， `build_depend` 和 `run_depend` 两个描述符分别指定了程序包编译和运行的依赖项，通常是所用到的库文件的名称。 在这里我们指定了三个编译和运行时依赖项，分别是`roscpp`（编写C++ ROS节点），`sensor_msgs`（定义了激光雷达的msg），`pcl_ros`（连接ROS和pcl库）。
