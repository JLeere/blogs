---
title: ubuntu常用问题记录
date: 2020-10-10 10:18:51
categories:
- 工具技能
tags:
- Ubuntu小操作
---

# 开机启动VPN脚本

每次开机都要设置鼠标灵敏度和打开ssVPN操作,不如让这两行命令开机启动,省事.

1.在`/etc/init.d/`文件夹下添加一个脚本文件vpn_open.sh, 

脚本格式如下:

```shell
#!/bin/sh
### BEGIN INIT INFO
# Provides:          vpn_open.sh
# Required-Start:    $local_fs $network
# Required-Stop:     $local_fs
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: open vpn service
# Description:       open vpn service HK2.json
### END INIT INFO
nohup ss-local -c /home/lee/socketpro/HK2.json &
xset m 2
```

2.将这个脚本添加到开机启动的服务中.[参考链接](https://blog.csdn.net/MakerCloud/article/details/81257953?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)

```shell
sudo chmod +x start_test.sh #设置权限
# 将脚本添加到启动脚本,90为优先级,数值越高优先级越低
cd /etc/init.d/
sudo update-rc.d start_test.sh defaults 90
# 移除服务方法
cd /etc/init.d
sudo update-rc.d -f start_test.sh remove
```

# 设置鼠标速度

```shell
xset m N #N为速度,2即可
```

# git clone

```shell
sudo chown lee:lee rslidar_sdk/ -R
```

