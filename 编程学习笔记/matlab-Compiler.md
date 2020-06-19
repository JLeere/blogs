---
title: matlab-Compiler
date: 2020-03-20 22:08:40
categories:
- 编程学习
- Matlab
tags:
- matlab
- MINGW
---

使用simulink或者coder常常需要C++的编译器，经常因为版本等问题报错。

# matlab不同模块需要的编译器版本
 https://ww2.mathworks.cn/support/requirements/previous-releases.html
 vc2012版本真的很低了，建议MINGW

# MINGW安装配置
1. [MINGW官网下载6.3版本](https://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win64/Personal%20Builds/mingw-builds/6.3.0/)
2. 下载安装教程 https://tieba.baidu.com/p/5487544851?pv=1 
下载好了直接解压即可
3. 注意不要把路径安装在带有中文字符和空格的文件夹下，尤其是**program files**
4. 环境变量：系统变量path中添加mingw路径（这是在电脑中添加C环境）；
    新建系统变量：MW_MINGW64_LOC，C:\MinGW
    ` setenv('MW_MINGW64_LOC','C:\mingw64\bin')` 这个好像就是matlab命令行中添加环境变量
5. 在matlab命令行中： `mex -setup` 查看编译器



    
