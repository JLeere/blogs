---
title: Ubuntu18.04
date: 2020-11-08 15：46：00
categories: 
- 工具技能
tag:
- Ubuntu
---

# Ubuntu 16.04

## 移动硬盘 Ubuntu 系统安装

[16.04见这篇文章](https://www.jaylee.top/2019/12/15/Linux%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85%E6%B3%A8%E6%84%8F/)

[18.04安装参考](https://www.jianshu.com/p/54d9a3a695cc)

## 文件目录

/ (根目录) >> / (home目录) >> ~ (当前用户目录) >> /下载 /桌面等

## 终端命令

命令格式:

>command  [-options]  [parameter] 
>sudo加在命令前面表示以管理员身份执行,如:
>sudo apt install/remove/upgrade *
>sudo apt-get update /upgrade
>cd (change directory) 相对/绝对路径(~ /)      cd .. 返回上层目录
>ls (list)/ll
>rm (remove)  删除之后无法恢复
>clear
>mkdir

<!-- more-->

# 操作系统准备

## 安装到移动硬盘

inference:https://www.jianshu.com/p/54d9a3a695cc

哎,在这种争分夺秒的毕业之际,我竟然也愿意花一个周六来玩这种东西,当作休息好啦. 不过18.04真香.

我是在新买的SSD中安装系统, 步骤还简单, 就是EFI安装的引导文件我放在了新建的系统的efi分区中了,导致win10每次都默认自己优先启动,所以每次都必须去F12界面选择要进入的系统.

1.  制作启动盘: 把下载下来的ios文件打开,复制所有文件内容到格式化的新U盘中就好了.
2. 修改BIOS启动顺序, 让USB先运行, 重启进入U盘里的系统
3. 安装Ubuntu18.04: 最主要的是分区, efiz主分区100M, /根目录主分区30G, /home目录逻辑分区70G, 引导文件放在了新建的efi分区上
4. 即可.

## 安装常用软件

1. chrome
2. typora
3. 网易云
4. vscode/插件
5. git/hexo

## 安装ROS Melodic

18.04Ubuntu不能装Kinetic了, 版本不对.未来可能会有一些驱动不支持

官网：http://wiki.ros.org/melodic/Installation/Ubuntu#Installation-1

## 安装VPN

我使用的是socketpro软件: https://www.socketprohc.com

按照帮助文档中的指示进行安装编译, 每次启动的时候使用nohup**&代码让他后台运行.

>[socketPro 使用手册](https://www.socketprohc.com/hc/zh-cn/articles/360022001091-安装Linux客户端)
>
>[开启vpn指令](https://www.cnblogs.com/kaituorensheng/p/3980334.html)

> 1. 即使关闭终端也可以后台进行:
>
> ```shell
> nohup ss-local -c ~/socketpro/HK2.json &
> ```
>
>  打开网络代理;
>
> 2. 显示后台进程: `ps -aux | grep "HK2.json"`
> 3. kill pid(进程号)
>
> 4. 关闭网络代理
>

## 安装proxychains

https://blog.popkx.com/Ubuntu-install-proxychains-let-terminal-using-socks5-proxy-speed-up-downloading/

代理网络, 加速终端下载。

用curl测试一直连不上服务器。也不知道proxychains有没有发挥作用，反正在费时慢速的命令前加上proxychains就可以使用了，心里作用感觉还行。

# 编程环境准备

## 安装Ceres-Solver

[入门百科](https://www.jianshu.com/p/e5b03cf22c80)

[官方文档](http://ceres-solver.org/features.html)

简单了解一下这是谷歌使用多年的用于求解非线性优化问题的库,在激光雷达SLAM算法中广泛被使用. 了解以下即可. 算法分为三步骤: 

1. 最优函数 
2. 构建待求解优化问题
3. 配置求解器参数

### 安装依赖库

```shell
# CMake >3.5 version:
sudo apt-get install cmake
# google-glog + gflags 用于记录有关内存分配和解决方案各个部分所消耗的时间，内部错误条件等的详细信息
sudo apt-get install libgoogle-glog-dev libgflags-dev
# BLAS & LAPACK
sudo apt-get install libatlas-base-dev
# Eigen3  > 3.3 version: 
sudo apt-get install libeigen3-dev
# SuiteSparse and CXSparse (optional)
sudo apt-get install libsuitesparse-dev
```

### 编译

```C++
tar zxf ceres-solver-2.0.0.tar.gz
mkdir ceres-bin
cd ceres-bin
cmake ../ceres-solver-2.0.0
make -j3
make test
# Optionally install Ceres, it can also be exported using CMake which
# allows Ceres to be used without requiring installation, see the documentation
# for the EXPORT_BUILD_DIR option for more information.
make install
```

## 安装ntcan驱动

[公众号连接](https://mp.weixin.qq.com/s?__biz=MzU0NTY1MjMyMA==&mid=2247483734&idx=1&sn=d34b05930faad591ab406f6980ef496d&chksm=fb68ea59cc1f634f059997a5df4d72245cf12482f3630387385f27bc857d8aac4bd34d6b6eac&mpshare=1&scene=1&srcid=0802y9aLyMoG1fLqLAmFckg8&sharer_sharetime=1564725138547&sharer_shareid=217e32ecaea6b07e5e13a33b8b664e23&exportkey=AUuW4VYfF46FRgrTQd9RpDY%3D&pass_ticket=3eWdTXq9a%2F0BR3GcSkFk2tt4qKx%2FRz3S6sUoOxcRmtXezeazDVg8%2BTmBGR12YNPo&wx_header=0#rd)

下载连接: http://esdshanghai.com/esd_download.html

自己电脑上调试只需要安装ntcan的头文件和源文件库即可,即文中的567步.

或者直接在CMake中注释掉can节点



