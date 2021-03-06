---
title: 安装Windows10+Ubuntu18.04双系统
date: 2019-01-14 16:47:32
tags: [Linux,Windows,Ubuntu]
categories: Tools
top:
description: Windows10+Ubuntu18.04双系统安装成功的记录
---
### Step:1 在硬盘上为Ubuntu在硬盘上创建一块空闲空间
登录Windows10系统，打开磁盘管理工具，我这里从D盘压缩出来了100G的空闲分区，保持此分区未分配；

<!-- more -->

### Step:2 制作Ubuntu安装镜像
 1. 下载Ubuntu18.04镜像文件
 2. 下载 Etcher 软件（用于制作一个可引导 Ubuntu 的 USB 驱动器）
 Etcher用于为任何 Linux 发行版创建可启动的介质的工具，我推荐 Etcher。Etcher 可以在三大主流操作系统（Linux、MacOS 和 Windows）上运行且不会让你覆盖当前操作系统的分区。
 3. 一旦你下载完成并运行 Etcher，点击选择镜像并指向你在步骤 4 中下载的 Ubuntu ISO 镜像， 接下来，点击驱动器以选择你的闪存驱动器，然后点击 “Flash!” 开始将闪存驱动器转化为一个 Ubuntu 安装器的过程。 （如果你正使用一个 DVD-R，使用你电脑中的 DVD 烧录软件来完成此过程。）

### Step:3 安装Ubuntu
**确保关闭了Windows系统的Secure Boot选项**。Secure Boot：安全启动，只可以启动Win8及以上系统，不能启动其他系统（包括USB、Linux）等。
此处有第一个重要提醒：网上很多教程教你如何给Linux分区，其实完全没有必要，Ubuntu已经聪明到会使用空闲的磁盘区域，自动按照需求分区。
选择`USB启动`
选择`Try Ubuntu`, 然后安装
选择`与现有系统共存`，
**此处有第一个重要提醒：网上很多教程教你如何给Linux分区，其实完全没有必要，Ubuntu已经聪明到会使用空闲的磁盘区域，自动按照需求分区。**
![](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/TIM20190114171716.jpg)
这里安装引导程序自动找到了刚刚创建的空闲分区

### Step:4 重新启动
选择你想进入的操作系统

**********************

下面是在Windows10的磁盘管理工具下面看到的情况，Ubuntu已经自动使用了D盘下面100个G的空闲空间。
![](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/TIM20190114165949.jpg)


Reference：
1. [How to install Ubuntu 18.04 in dual boot alongside Windows 10](http://myviewsonfoss.blogspot.com/2018/05/this-article-willshow-you-how-you-can.html)
2. [如何实现Linux+Windows双系统启动](https://weibo.com/ttarticle/p/show?id=2309404315285027958947#_0)
