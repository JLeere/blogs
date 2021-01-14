---
title: ubuntu18.04扩大交换空间
date: 2021-01-09 12:35:01
categories:
- 工具技能
tags:
- Ubuntu
---

当时建系统的时候交换空间给了1G，太小了，导致编译程序的时候一旦RAM满了就会卡死，我不得不用-j1命令限制编译的线程数，这样很慢。

因而我们可以扩大交换空间，这样，内存满的时候他可以自动把一些静态的缓存放到这里面避免卡死。

我也是小白，试了很多教程，就这一篇是最好的，能够成功。

https://www.howtoing.com/how-to-add-swap-space-on-ubuntu-18-04，转载保存如下：

《如何在Ubuntu 18.04上添加交换空间》

防范应用程序内存不足错误的最简单方法之一是为服务器添加一些交换空间。在本指南中，我们将介绍如何将交换文件添加到Ubuntu 18.04服务器。

分类:[Ubuntu](https://www.howtoing.com/category/ubuntu)[Linux基础](https://www.howtoing.com/category/linux-basics)[操作系统](https://www.howtoing.com/category/operating-system)[Ubuntu 18.04](https://www.howtoing.com/category/ubuntu-18-04)

 *2018-07-05 15:24:21*

*本教程的先前版本由[Justin Ellingwood](https://www.digitalocean.com/community/users/jellingwood)编写*

防范应用程序内存不足错误的最简单方法之一是为服务器添加一些交换空间。 在本指南中，我们将介绍如何将交换文件添加到Ubuntu 18.04服务器。

**警告：**虽然通常建议对使用传统旋转硬盘驱动器的系统进行交换，但使用SSD交换可能会导致硬件随着时间的推移而出现问题。 出于这种考虑，我们不建议在DigitalOcean或任何其他使用SSD存储的提供商上启用交换。 这样做会影响您和您的邻居的底层硬件的可靠性。 本指南是为可能在其他地方使用旋转磁盘系统的用户提供的参考。

如果您需要在DigitalOcean上提高服务器的性能，我们建议您升级Droplet。 这将导致更好的结果，并将降低导致可能影响您的服务的硬件问题的可能性。

## 什么是交换？

*交换*是硬盘驱动器上的一个区域，它被指定为操作系统可以临时存储无法再保存在RAM中的数据的位置。 基本上，这使您能够增加服务器在其工作“内存”中可以保留的信息量，但有一些注意事项。 硬盘驱动器上的交换空间将主要用于RAM中不再有足够空间来保存正在使用的应用程序数据。

写入磁盘的信息将比保存在RAM中的信息慢得多，但操作系统更愿意在内存中运行应用程序数据并使用交换旧数据。 总的来说，将交换空间作为系统RAM耗尽时的后备空间，可以很好地防范具有非SSD存储的系统上的内存不足异常。

## 第1步 - 检查系统的交换信息

在开始之前，我们可以检查系统是否已经有一些可用的交换空间。 可以有多个交换文件或交换分区，但通常一个就足够了。

我们可以通过键入以下内容来查看系统是否已配置任

```
sudo swapon --show
```

如果您没有收到任何输出，这意味着您的系统当前没有可用的交换空间。

您可以使用`free`实用程序验证没有活动交换：

```
free -h
Output              total        used        free      shared  buff/cache   available
Mem:           985M         84M        222M        680K        678M        721M
Swap:            0B          0B          0B
```

正如您在输出的**Swap**行中所看到的，系统上没有活动交换。

## 第2步 - 检查硬盘驱动器分区上的可用空间

在我们创建交换文件之前，我们将检查当前的磁盘使用情况，以确保我们有足够的空间。 通过输入：

```
df -h
OutputFilesystem      Size  Used Avail Use% Mounted on
udev            481M     0  481M   0% /dev
tmpfs            99M  656K   98M   1% /run
/dev/vda1        25G  1.4G   23G   6% /
tmpfs           493M     0  493M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           493M     0  493M   0% /sys/fs/cgroup
/dev/vda15      105M  3.4M  102M   4% /boot/efi
tmpfs            99M     0   99M   0% /run/user/1000
```

在这种情况下，带有`/`在`Mounted on`列上的设备是我们的磁盘。 在这个例子中我们有足够的空间（仅使用1.4G）。 您的用法可能会有所不同。

虽然对交换空间的适当大小有很多意见，但这实际上取决于您的个人偏好和您的应用程序要求。 通常，等于或加倍系统RAM的量是一个很好的起点。 另一个好的经验法则是，如果您只是将其用作RAM后备，那么任何超过4G的交换都可能是不必要的。

## 第3步 - 创建交换文件

现在我们知道了可用的硬盘空间，我们可以在文件系统上创建一个交换文件。 我们将在根（/）目录中分配一个我们想要的交换大小的文件，称为`swapfile` 。

创建交换文件的最佳方法是使用`fallocate`程序。 此命令立即创建指定大小的文件。

由于我们示例中的服务器具有1G的RAM，因此我们将在本指南中创建1G文件。 调整此项以满足您自己的服务器的需求：

```
sudo fallocate -l 1G /swapfile
```

我们可以通过输入以下内容来验证是否保留了正确的空间量：

```
ls -lh /swapfile
-rw-r--r-- 1 root root 1.0G Apr 25 11:14 /swapfile
```

我们的文件创建时留出了正确的空间。

## 第4步 - 启用交换文件

现在我们有一个正确大小的文件，我们需要实际将其转换为交换空间。

首先，我们需要锁定文件的权限，以便只有具有**root**权限的用户才能读取内容。 这可以防止普通用户访问该文件，这会产生重大的安全隐患。

通过键入以下内容使该文件只能由**root**访问：

```
sudo chmod 600 /swapfile
```

键入以下命令验证权限更改：

```
ls -lh /swapfile
Output-rw------- 1 root root 1.0G Apr 25 11:14 /swapfile
```

如您所见，只有**root**用户启用了读写标志。

我们现在可以通过输入以下内容将文件标记为交换空间

```
sudo mkswap /swapfile
OutputSetting up swapspace version 1, size = 1024 MiB (1073737728 bytes)
no label, UUID=6e965805-2ab9-450f-aed6-577e74089dbf
```

标记文件后，我们可以启用交换文件，允许我们的系统开始使用它：

```
sudo swapon /swapfile
```

输入以下内容验证交换是否可用：

```
sudo swapon --show
OutputNAME      TYPE  SIZE USED PRIO
/swapfile file 1024M   0B   -2
```

我们可以再次检查`free`工具的输出以证实我们的发现：

```
free -h
Output              total        used        free      shared  buff/cache   available
Mem:           985M         84M        220M        680K        680M        722M
Swap:          1.0G          0B        1.0G
```

我们的交换已成功设置，我们的操作系统将在必要时开始使用它。

## 第5步 - 使交换文件永久化

我们最近的更改已启用当前会话的交换文件。 但是，如果我们重新启动，服务器将不会自动保留交换设置。 我们可以通过将交换文件添加到`/etc/fstab`文件来更改此设置。

备份`/etc/fstab`文件，以防出现任何问题：

```
sudo cp /etc/fstab /etc/fstab.bak
```

键入以下命令，将交换文件信息添加到`/etc/fstab`文件的末尾：

```
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

接下来，我们将查看一些我们可以更新的设置以调整我们的交换空间。

## 第6步 - 调整您的交换设置

您可以配置一些选项，这些选项会在处理交换时对系统的性能产生影响。

### 调整Swappiness属性

`swappiness`参数配置系统将数据从RAM交换到交换空间的频率。 这是介于0和100之间的值，表示百分比。

值接近于零时，除非绝对必要，否则内核不会将数据交换到磁盘。 请记住，与交换文件的交互是“昂贵的”，因为它们比与RAM的交互花费更长的时间，并且它们可能导致性能的显着降低。 告诉系统不要太依赖交换通常会使您的系统更快。

接近100的值将尝试将更多数据放入交换中以努力保持更多RAM空间。 根据应用程序的内存配置文件或服务器的使用情况，在某些情况下可能会更好。

我们可以通过输入以下内容来查看当前的swappiness值：

```
cat /proc/sys/vm/swappiness
Output60
```

对于桌面，swappiness设置为60并不是一个糟糕的值。 对于服务器，您可能希望将其移近0。

我们可以使用`sysctl`命令将swappiness设置为不同的值。

例如，要将swappiness设置为10，我们可以键入：

```
sudo sysctl vm.swappiness=10
Outputvm.swappiness = 10
```

此设置将持续到下次重新引导。 我们可以通过在`/etc/sysctl.conf`文件中添加该行来自动设置此值：

```
sudo nano /etc/sysctl.conf
```

在底部，您可以添加：

/etc/sysctl.conf中

```
vm.swappiness=10
```

完成后保存并关闭文件。

### 调整缓存压力设置

您可能想要修改的另一个相关值是`vfs_cache_pressure` 。 此设置配置系统将选择多少缓存*inode*和*dentry*信息而不是其他数据。

基本上，这是关于文件系统的访问数据。 这通常是非常昂贵的查询和非常频繁的请求，所以这是你的系统缓存的一个很好的事情。 您可以通过再次查询`proc`文件系统来查看当前值：

```
cat /proc/sys/vm/vfs_cache_pressure
Output100
```

由于它当前已配置，我们的系统会过快地从缓存中删除inode信息。 我们可以通过键入以下内容将其设置为更保守的设置（如50）：

```
sudo sysctl vm.vfs_cache_pressure=50
Outputvm.vfs_cache_pressure = 50
```

同样，这仅适用于我们当前的会话。 我们可以通过将其添加到配置文件来改变它，就像我们使用swappiness设置一样：

```
sudo nano /etc/sysctl.conf
```

在底部，添加指定新值的行：

/etc/sysctl.conf中

```
vm.vfs_cache_pressure=50
```

完成后保存并关闭文件。

## 结论

按照本指南中的步骤将为您提供一些Wheezy空间，否则会导致内存不足异常。 交换空间在避免一些常见问题方面非常有用。

如果遇到OOM（内存不足）错误，或者如果发现系统无法使用所需的应用程序，最佳解决方案是优化应用程序配置或升级服务器。