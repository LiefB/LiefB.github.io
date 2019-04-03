---
layout:     post
title:      win10+ubuntu双系统安装
subtitle:   双系统安装
date:       2019-04-03
author:     LiefB
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Linux
---



> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://segmentfault.com/a/1190000014523888

这篇博客可以解决
**1\. 如何安装 win10,ubuntu 双系统**
**2\. 如何使用 win10 引导 Ubuntu，并且设置 win10 引导界面**

# win10,ubuntu 双系统的安装

为什么要装双系统，之前用的虚拟机，但是虚拟机没有显卡，使用 gazebo 之类的 3D 仿真软件经常出问题，所以选择装双系统。
强烈建议安装 **Ubuntu16.04 的 64 位系统**，可以使你之后的开发环境搭建非常顺利。

## 0\. 工具

硬件：一个空的 U 盘 (4G 及以上)
软件：[EasyBCD2.3](https://pan.baidu.com/s/1lwPUM-a_Vi_cgVe3WC264g) 密码 tb8w, [傲梅分区助手](https://www.disktool.cn/download.html),[UltraISO](http://cn.ultraiso.net/xiazai.html),[Ubuntu 系统](https://www.ubuntu.com/download/desktop)

## 1\. 创建 Ubuntu 分区

### 1）使用 win10 自带的磁盘管理

按住 Win + X，选择 “磁盘管理”
![](https://segmentfault.com/img/remote/1460000014523891?w=839&h=591)
选择剩余空间较大的可分配磁盘，右键并选择 “压缩卷”，这里选择压缩 E 盘 50G 左右的空间
![](https://segmentfault.com/img/remote/1460000014523892?w=842&h=593)
点击 “压缩” 之后，E 盘后部出现黑色的 50G“未分配空间”
![](https://segmentfault.com/img/remote/1460000014523893?w=841&h=593)
至此，磁盘分区过程完成. 这种方法参考[相关博客](https://blog.csdn.net/pop_rain/article/details/70477085)

### 2）使用傲梅分区助手

![](https://segmentfault.com/img/remote/1460000014523894?w=1222&h=772)

> **给磁盘分区有很多种方法，可以自行选择，分区的目的是为了给 Ubuntu 系统分配安装空间，建议 40G 及以上。**

## 2\. 制作 Ubuntu 的启动 U 盘

**制作启动 U 盘会格式化 U 盘，注意备份文件。**
将 U 盘其插入电脑, 进入 UltraISO，
![](https://segmentfault.com/img/remote/1460000014523895?w=829&h=506)

打开下载好的 Ubuntu 镜像文件 (ubuntu ISO 文件)。
![](https://segmentfault.com/img/remote/1460000014523896?w=832&h=507)

保持默认设置即可
![](https://segmentfault.com/img/remote/1460000014523897?w=543&h=506)

点击写入
![](https://segmentfault.com/img/remote/1460000014523898?w=540&h=237)
Ubuntu 的启动 U 盘制作完成，其实非常简单，就是打开软件，选择镜像文件，点击写入即可。[图片来源博客](https://blog.csdn.net/pop_rain/article/details/70477085)

## 3\. 安装系统

首先需要进入 bios 设置优先 u 盘启动。
不同品牌的电脑进入 bios 的方式不同，可以自行百度。
在 bios 中找到 USB HDD 选项用 Tab 键把它调整到第一的位置即可
![](https://segmentfault.com/img/remote/1460000014523899?w=4640&h=2610)

设置完成后，插入 U 盘然后重启电脑，就会进入系统安装界面

![](https://segmentfault.com/img/remote/1460000014523900?w=1011&h=745)
建议都不要选，否者会需要联网下载，会很慢。
![](https://segmentfault.com/img/remote/1460000014523901?w=873&h=607)

<font color=red> 注意这里选其他选项 </font>
![](https://segmentfault.com/img/remote/1460000014523902?w=873&h=607)

点击的 “+” 创建 4 个主要的基础分区（这里之前未分配的 50G 就是给 ubuntu 系统的 50G），按以下参数设置 4 个主要的基础分区：

| 大小 | 新分区的类型 | 新分区的位置 | 用于 | 挂载点 | 用途 |
| --- | --- | --- | --- | --- | --- |
| 10G | 主分区 | 空间起始位置 | Ext4 日志文件系统 | / | 用于存放系统相当于 win10 的 C 盘 |
| 4G | 逻辑分区 | 空间起始位置 | 交换空间 | /swap | 相当于电脑内存 |
| 200MB | 逻辑分区 | 空间起始位置 | Ext4 日志文件系统 | /boot | 引导分区 |
| 所有剩余的空间 | 逻辑分区 | 空间起始位置 | Ext4 日志文件系统 | /home | 用户存储数据用 |

> 我的系统安装完后 主分区使用了 1.31G,boot 分区使用了 151.72M, 所以如果你硬盘有限至少也要大于这个标准。

#### 主分区 /

![](https://segmentfault.com/img/remote/1460000014523903?w=799&h=685)

#### 交换空间 /swap

![](https://segmentfault.com/img/remote/1460000014523904?w=795&h=677)

#### 引导分区 /boot

![](https://segmentfault.com/img/remote/1460000014523905?w=795&h=689)

#### 用户数据 /home

![](https://segmentfault.com/img/remote/1460000014523906?w=795&h=692)

### 用 win 引导 Ubuntu！

<font color=red> 安装启动引导设备的参数选择：与 / boot 所在的编号一致。</font>
**如果这步忽略了，你就用了 Ubuntu 系统来引导 Windows 了**
![](https://segmentfault.com/img/remote/1460000014523907?w=783&h=685)

之后就没坑了
![](https://segmentfault.com/img/remote/1460000014523908?w=789&h=685)
![](https://segmentfault.com/img/remote/1460000014523909?w=795&h=685)
记住你的密码别忘了。
![](https://segmentfault.com/img/remote/1460000014523910?w=795&h=680)

> 到这里 Ubuntu 已经安装好了，可以重启，然后拔掉 U 盘，如果你按照我的步骤，重启会进入 **win10 系统**。

## 4\. 设置 win10 引导 Ubuntu

一定要用 EasyBCD2.3
重启在 win10 下进入 EasyBCD，选择 “添加新条目”，选择 Linux/BSD 操作系统，在“驱动器” 栏目选择接近 200M（就是 boot 的分区）的 Linux 分区，点添加条目
![](https://segmentfault.com/img/remote/1460000014523911?w=558&h=446)
编辑引导菜单，勾选 Use Metro bootloader
![](https://segmentfault.com/img/remote/1460000014523912?w=711&h=602)

重启电脑，恭喜你，Win10 和 Ubuntu 的双系统已经完成安装
![](https://segmentfault.com/img/remote/1460000014523913?w=1240&h=930)

> 用 Windows 引导 Ubuntu 最大的好处就是，当不再需要 Ubuntu 的时候，直接在 Windows 磁盘管理中将其所在所有分区删除，然后将 EasyBCD 中对应条目删除即可。

写教程真心不容易，感谢网络上分享教程的大神，[折腾日记] 这个栏目以后用来记录一些教程工具的折腾过程，目的是同样的坑绝不踩两遍。如果碰巧解决了你的问题，请点赞转发分享一下哟。

预告：系统装好了接下来当然是为 Ubuntu 设置科学上网，然后安装 Pixhawk 编译环境，很快更新，尽情期待。

# 踩过的坑

引导设置错误，重启就会进入这个界面，这是 Ubuntu 引导 win10, 如果你在这种情况删除 Ubuntu 的分区，你会发现 win10 也进不去了，需要修复 win10 的引导。我为此又重装了一遍。。。。。
![](https://segmentfault.com/img/remote/1460000014523914?w=763&h=473)
重启进入 win10 但是界面太丑，因为用了 EasyBCD2.2 没有 Use Metro bootloader 选项
![](https://segmentfault.com/img/remote/1460000014523915?w=580&h=435)

参考资料
[https://www.cnblogs.com/zhuyi...](https://www.cnblogs.com/zhuyinxiaozi/p/5424284.html)
[https://blog.csdn.net/pop_rai...](https://blog.csdn.net/pop_rain/article/details/70477085)
[https://jingyan.baidu.com/art...](https://jingyan.baidu.com/article/3c48dd348bc005e10be358eb.html)