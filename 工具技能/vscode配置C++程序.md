---
title: vscode调试运行cpp程序
date: 2020-10-12 20:05:29
categories:
- 工具技能
- vscode
tags:
- vscode
---



与文档"vscode调试运行ROS程序.md"相似,本文档简单讲讲不使用Cmake配置C++/gcc环境.以简单程序调用Eigen库为例.

# 编写代码 

```C++
//mypg.cpp
#include<iostream>
#include <Eigen/Dense>

using Eigen::MatrixXd;

int main(){
    MatrixXd m(2,2);
    m(0,0) = 3;
    m(1,0) = 2.5;
    m(0,1) = -1;
    m(1,1) = m(1,0) + m(0,1);
    std::cout << m << std::endl;
}
```

# 编译和链接

先要将文档编译成可执行文件,但是需要链接Eigen库头文件,可利用`gcc -I`指令在终端编译

```C++
gcc -I /usr/include/eigen3/ mypg.cpp -o mypg
```

## Task.json

而在vscode中, 可按`ctrl+shift+b`或F7编译. `ctrl+shift+P`输入Task指令生成*task.json*, 修改task.json中的编译相关参数.

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "type": "shell",
            "label": "build",
            "command": "/usr/bin/g++",		//终端运行的指令
            "args": [ //此项是上述命令G++的参数列表
                "-g",
                "${fileDirname}/*.cpp",
                //"${workspaceFolder}/src/*.cpp",	//填写需要编译的目标cpp文件
                
                "-o",
                "${fileDirname}/${fileBasenameNoExtension}.exe",
                //生成的可执行文件存放的目录
                //${fileDirname}当前工作目录; ${fileBasenameNoExtension}以活动文件名为基础的没有扩展名的可执行文件
                
                "-std=c++11",
                "-stdlib=libc++",	//这两句是默认的编译器C++98更改为C++11
                
                "-I",   //头文件链接目录.实践证明不好用,在c_cpp_properties中includePath设置更好.ctrl+shift+p配置C/C++.
                //"${workspaceFolder}/include ""
                "/usr/include/eigen3/"
            ],
            "options": {
                "cwd": "${workspaceFolder}" //the task runner's current working directory on startup
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": "build"
        },
    ]
}
```

但是我们发现并没有链接上eigen库,因此需要配置C/C++ Configrations.

## c_cpp_properties.json

`ctrl+shift+P`调出*C/C++ Configrations.* 在includePath中添加包含路径

```C++
{
    "configurations": [
        {
            "name": "Linux",
            "includePath": [
                "${default}",
                "${workspaceFolder}/**",
                "/usr/include/eigen3/"
            ],
            "defines": [],
            "compilerPath": "/usr/bin/gcc",
            "cStandard": "gnu11",
            "cppStandard": "c++17",
            "intelliSenseMode": "gcc-x64"
        }
    ],
    "version": 4
}
```

这样就可以编译了,`ctrl+shift+B`. 生成相应的可执行文件`mypg.exe`

# 调试运行

按F5自动生成调试用的**launch.json**文档.

要清楚调试是基于生成的可执行文件*.exe才能调试, 因此在先没有可执行文件情况下需要设置**preLaunchTask**为自己的编译任务的名字. 

可以定义多个调试器, 以运行不同的cpp和参数调试.

```json
{
    // 使用 IntelliSense 了解相关属性。 
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "g++ - 生成和调试活动文件",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}/${fileBasenameNoExtension}.exe",//注意可执行文件的后缀
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
            "preLaunchTask": "build",
            "miDebuggerPath": "/usr/bin/gdb"
        }
    ]
}
```



