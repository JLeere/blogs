---
title: Cmake(一)
date: 2020-10-05 20:20:25
updated: 2020-10-19 12:00:00
categories:
- 编程学习
- Cmake
tags:
- CMake
top: #100
---

### CmakeList

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