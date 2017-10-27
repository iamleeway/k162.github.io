---
layout: post
title: 安装 WIN10 ubuntu 双系统 避坑指南
tags: linux tips tutorial
key: 10022
date: 2017-10-27 00:00:00 +08:00
---

> 虚拟机的系统毕竟有局限性，比如不支持 GPU 计算，必要的时候还得硬安装个系统，正所谓技能都是折腾出来的，在此整理一份靠谱的 win10 & ubuntu 双系统安装与避坑指南。 

<!--more-->

## 一、安装前的准备工作

### 1. 压缩空间预留分区

- 进行磁盘清理  
    这样做是为了便于压缩出磁盘空间（如果安装到新的硬盘则无需该操作）  
    选择想要压缩的磁盘，点击`优化`即可  
    ![](http://ors3vio5q.bkt.clouddn.com/17-10-27/95147729.jpg) 

- 进入磁盘管理  
    1. 选择想要压缩的磁盘，在下方右键选择`压缩卷`  
    ![](http://ors3vio5q.bkt.clouddn.com/17-10-27/72147014.jpg)
    1. 输入想要安装的新的分区的空间（不建议低于25G）  
    ![](http://ors3vio5q.bkt.clouddn.com/17-10-27/45771251.jpg)
    1. 完成之后就多了一个未分配的空间  
    ![](http://ors3vio5q.bkt.clouddn.com/17-10-27/19832788.jpg)

### 2. 下载系统镜像 

![](http://ors3vio5q.bkt.clouddn.com/17-10-27/78087853.jpg)

- 下载地址：<https://www.ubuntu.com/download/desktop> 

- 选择 `Ubuntu 16.04.3 LTS` （长期支持版） 或者 `Ubuntu 17.10` （最新的）都可以 

### 3. 刻录到 U盘中 

1. 下载 [Rufus U盘刻录工具](https://rufus.akeo.ie/?locale=zh_CN) 

1. 选择镜像，按下图设置然后刻录即可  
![](http://ors3vio5q.bkt.clouddn.com/17-10-27/35455234.jpg)

### 4. 设置 U盘启动（BIOS） 
这个网上资料非常多，简要说下：一般进入 BIOS 是开机按 F2（笔记本居多） 或 F10 或 DEL（台式机居多），开机启动顺序一般在 Advanced BIOS Features 中设置，里面有开机顺序选择，把 USB 选项放到硬盘启动之前即可。 

## 二、进行系统安装 
1. 选择语言，然后点击 Install 进行安装  
![](http://ors3vio5q.bkt.clouddn.com/17-10-27/16386801.jpg)

1. 设置安装更新和驱动（不赶时间的可以都勾选）  
![](http://ors3vio5q.bkt.clouddn.com/17-10-27/17286168.jpg)

1. 选择安装类型  
第一个表示和 WIN10 安装在同一个分区（不推荐）  
我们要在新的分区安装，**选择最后一项**`do something else`  
![](http://ors3vio5q.bkt.clouddn.com/17-10-27/79399326.jpg)

1. 挂载分区  
![](http://ors3vio5q.bkt.clouddn.com/17-10-27/41458940.jpg)
很多人在这个步骤搞不清楚，其实原理非常简单，即你准备把目前的可用磁盘空间如何进行分配（和“分区”有点类似），我们一般只需要关注两种：系统分区和 SWAP 分区。  
    - **SWAP 分区**，可以把这个看作虚拟内存（一般设置为物理内存的 1.5 ~ 2 倍）。当物理内存耗尽时，才会用到 swap，因此如果你内存够大，比如 16G，是完全可以不需要 swap 分区的  
    ![](http://ors3vio5q.bkt.clouddn.com/17-10-27/82256506.jpg)
    - **系统分区**，大部分空间挂给系统即可  
    ![](http://ors3vio5q.bkt.clouddn.com/17-10-27/91051437.jpg)

1. 设置完毕进行安装，设置时区、用户名等 
![](http://ors3vio5q.bkt.clouddn.com/17-10-27/92391780.jpg)
![](http://ors3vio5q.bkt.clouddn.com/17-10-27/17902599.jpg)
![](http://ors3vio5q.bkt.clouddn.com/17-10-27/26486827.jpg)

1. 安装完毕后重启即可  
![](http://ors3vio5q.bkt.clouddn.com/17-10-27/82821297.jpg)
![](http://ors3vio5q.bkt.clouddn.com/17-10-27/52786370.jpg)

## 三、系统设置：调整引导 

- 默认的引导界面是这样的  
![](http://ors3vio5q.bkt.clouddn.com/17-10-27/76479014.jpg)

- 如果想设置 WIN10 优先，可以编辑 grub 文件  
`sudo vi /etc/default/grub`，然后根据自己开机显示的操作选项顺序设置 DEFAULT 的值（0开始计数） 

- 当然，可以选择更加图形化的操作方法，安装 grub-customizer  

```bash 
sudo add-apt-repository ppa:danielrichter2007/grub-customizer 
sudo apt-get update 
sudo apt-get install grub-customizer 
``` 

- 使用 grub-customizer 调整顺序即可  
![](http://ors3vio5q.bkt.clouddn.com/17-10-27/82149141.jpg)

- 调整引导之后的效果  
![](http://ors3vio5q.bkt.clouddn.com/17-10-27/58481060.jpg)

## 四、卸载系统与恢复引导 
如果 ubuntu 出了问题，或者厌倦了，恐怕又得折腾回去，在此一并分享下备用吧。 

在双系统安装完成之后，默认是用 ubuntu 引导 windows，想要卸载系统，得先恢复引导。 

1. 进入 windows，下载 [EasyBCD](http://neosmart.net/EasyBCD/) 

1. 将 windows 作为主引导（MBR）  
![](http://ors3vio5q.bkt.clouddn.com/17-10-27/86495946.jpg)

1. 再进入磁盘管理，删除 ubuntu 分区，然后并入原来的分区即可。 

## 避坑指南 

### 1. U盘启动不识别 
用 Win32DiskImager 制作启动盘试试，或者换个 U盘。 

### 2. U盘启动，安装界面点击 install ubuntu 黑屏 
这是由于不识别显卡的问题，这里以 N 卡为例： 

1.  在 `install ubuntu` 选项，按`e` 
2.  删掉`---`，添加 `nomodeset` 

### 3. 安装完毕后，启动 ubuntu 系统黑屏 
这依然是由于不识别显卡的问题，需要进入系统修改： 

- 先进入系统 
    1. 在启动 `ubuntu` 选项，按`e` 
    1. 先在`quiet splash`后面添加 `nomodeset`，然后按`ctrl + x` 进入系统 

- 再修改 grub 
    1. `sudo vi /etc/default/grub` 
    1. `GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"`这行修改为`GRUB_CMDLINE_LINUX_DEFAULT="quiet splash nomodeset"` 
    1. 更新GRUB：`sudo update-grub` 

### 4. 引导界面，原来的 WIN10 系统不见了 
这是因为 grub 文件的问题，不用担心 windows 系统没了。 

进入系统后，执行 `sudo update-grub` 应该就可以解决。 

若还是不行：[参考1](http://blog.csdn.net/zhanghuoding/article/details/42588241)、[参考2](http://www.cnblogs.com/hanggegege/p/6056750.html)、[参考3](https://askubuntu.com/questions/435160/cant-boot-windows-7-after-installing-ubuntu) 

## 参考 
- <https://www.youtube.com/watch?v=qNeJvujdB-0>
- <http://www.jianshu.com/p/30795695be95>

