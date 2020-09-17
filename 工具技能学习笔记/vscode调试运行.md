---
title: ROS/vscode调试运行
date: 2020-06-17 19:55:25
categories:
- 工具技能学习
- ROS
tags:
- ROS
- vscode
---

# lidar_perception 调试
## Github
从Chen的仓库中克隆最新分支dev,并转移到自己新建的分支dev_Lee

```
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

# vscode编辑器下调试ROS程序配置
[阅读vscode官方插件cmake tools说明文档](https://vector-of-bool.github.io/docs/vscode-cmake-tools/index.html)

## 摘录
>选择kit。Kit是一组工具包，包括编译器、链接器等其他工具，如gcc,clang等

>variant。有4种，debug,release是其中两种。


## 编译 
vscode安装插件：ROS, C/C++, C++ Intellisense, CMake Rools, 
终端中打开vscode当前目录`code .`，会自动生成‘.vscode'文件夹,里面包含两个.json配置文件:
>c_cpp_properties.json 主要是ROS插件生成,用于识别配置ros相关头文件等
>settings.json vscode编辑器设置文档

通过另一个配置文件task.json, 这里可以直接在vscode进行catkin_make。ctrl+shift+P调出vscode的命令行，输入Task:Config default task生成*.json文件。修改其内容见链接内容，主要是配置catkin_make或者g++编译的参数。
编译后出现compile_commands.json，
`catkin_make -DCMAKE_EXPORT_COMPILE_COMMANDS=Yes`
这个文件可以帮助我们关联编译所需要的文件路径，需要添加在c_cpp_properties.json里面"compileCommond"配置中
`"compileCommands":"${workspaceFolder}/build/compile_commands.json"`

### **c_cpp_properties.json**

```json
{//c_cpp_properties.json
    "configurations": [
        {
            "browse":{
                "databaseFilename":"",
                "limitSymbolsToIncludedHeaders":true
            },
            "includePath": [
                "/opt/ros/kinetic/include/**",
                "/usr/include/**"
            ],
            "name": "ROS",
            "intelliSenseMode": "gcc-x64",
            "compilerPath": "/usr/bin/gcc",
            "cStandard": "gnu11",
            "cppStandard": "c++17",
            "configurationProvider": "ms-vscode.cmake-tools", 
            "compileCommands": "${workspaceFolder}/build/compile_commands.json"
            //上述两行可解决找不到ros.h的类似问题
        }
    ],
    "version": 4
}
```
### **task.json**

```json
{//task.json chen
	"version": "2.0.0",
	"tasks": [
		{
			"type": "shell",
			"label": "catkin_make",
			"command": "catkin_make",
			"args": [
				"-j4",
				"-DCMAKE_BUILD_TYPE=Release",
				"-DCMAKE_EXPORT_COMPILE_COMMANDS=Yes",
				"-DCMAKE_CXX_STANDARD=14"
			],
			
			"problemMatcher": [
				"$catkin-gcc"
			],
			"group": {
				"kind": "build",
				"isDefault": true
			}
		}
	]
}
```
```json
{//task from Bilibili
	"version": "2.0.0",
	"tasks": [
		{
			"type": "shell",
			"label": "build",
			"command": "source /opt/ros/kinetic/setup.bash && catkin_make -DCMAKE_BUILD_TYPE=Debug",
			"args": [],
			"options": {
				"cwd": "${workspaceFolder}"
			},
			"problemMatcher": [
				"$catkin-gcc"
			],
			"group": {
				"kind": "build",
				"isDefault": true
			}
		},
		{
			"type": "shell",
			"label":"release",
			"command":"source /opt/ros/kinetic/setup.bash && catkin_make -DCMAKE_BUILD_TYPE=Release",
			"problemMatcher": [
				"$catkin-gcc"
			],
			"group": "build",
		}

	]
}
```



以上就可以和在终端中一样运行程序了。但是想要设置断点对程序进行调试debug则需要更多配置，生成debug版本的可执行程序。
后话：实际上还是会提示找不到ros.h.是不是ws目录必须在～下？

## GDB debug
>GDB调试器是调试C++代码的神器，ROS项目本质上也是一个ROS项目，因此也可以用GDB进行调试
>在vscode里面已经继承了GDB调试器，我们需要做的就是配置launch.json文件

点击左侧工具栏调试按钮，自动生成launch.json.

[B站视频链接](https://www.bilibili.com/video/BV1Ft411M7Uk)

```json
{
    // 使用 IntelliSense 了解相关属性。 
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb)Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/devel/lib/tutorials/talker", 
            //修改，将需要调试的节点在编译后生成的可执行文件的路径添加
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "为 gdb 启用整齐打印",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "build",//若没有修改就不需要每次都编译.也可以每次调试前先catkin_make
        }
    ]
}
```







