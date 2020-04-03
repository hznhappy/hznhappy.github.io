---
layout:     post
title:      "VirtualBox虚拟机Ubuntu系统服务器版本添加共享文件夹"
subtitle:   "Ubuntu服务器系统命令使用"
date:       2020-04-02
author:     "Jannon"
header-img: "images/WebHeader/computer-technology-business-website-header.jpg"
tags:
    - Ubuntu
---

## VirtualBox虚拟机Ubuntu服务器版本安装增强功能
Ubuntu服务器版本没有系统界面，所有功能都需要使用命令操作，[VirtualBox](https://www.virtualbox.org/wiki/Downloads) 虚拟机上安装了Ubuntu系统服务器版本之后，需要和主机系统共享数据，需要共享主机文件夹给虚拟机上的Ubuntu服务器系统，首先需要安装virtualbox的增强功能，点击菜单安装增强功能之后，需要在系统里面加载增强功能的ISO文件内容进行安装，安装步骤如下：
```
1、启动VirtualBox
2、启动虚拟机上Ubuntu服务器系统
3、点击‘设备-安装增强功能’
4、使用命令`sudo mount /dev/cdrom /media/`挂载增强功能CD-ROM设备到/media目录
6、使用命令`cd /media`切换到media目录
7、安装必要的一些依赖，命令`sudo apt-get install -y dkms build-essential linux-headers-generic linux-headers-$(uname -r)`
8、安装增强功能，命令`sudo ./VBoxLinuxAdditions.run`
9、安装完成，命令`modinfo vboxguest`可查看安装完成的增强功能模块信息
```
## 添加共享文件夹
virtualbox设置里面设置好要共享到虚拟机的文件夹，如VShare，设置好之后，虚拟机中输入如下命令挂载共享文件夹到指定虚拟机目录下，命令`sudo mount -t vboxsf VShare /mnt`即可完成挂载共享文件夹到/mnt目录，该目录只有root和用户组vboxsf才有读写权限，添加当前用户到vboxsf用户组，`sudo usermod -aG vboxsf username`，切换到共享目录`sudo su`和`cd /mnt`即可访问共享文件夹内容。

## 错误修复
如果安装增强功能发现不起作用，安装过程命令行有如下提示：
```
This system is currently not set up to build kernel modules.
Please install the gcc make perl packages from your distribution.
VirtualBox Guest Additions: Running kernel modules will not be replaced until
the system is restarted
```
则需要安装gcc、make和perl依赖库才能正常安装增强功能，安装命令：
``` bash
sudo apt-get install build-essential gcc make perl dkms
reboot
```
由于之前已经安装了build-essential和dkms，则只需要`sudo apt-get install gcc make perl`即可。
## Ubuntu国内源
```
备份/etc/apt/sources.list
#备份
cp /etc/apt/sources.list /etc/apt/sources.list.bak

在/etc/apt/sources.list文件前面添加如下条目
#添加阿里源
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse

执行如下命令更新源
#更新
sudo apt-get update
sudo apt-get upgrade

其他国内源如下：
#中科大源
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse

#163源
deb http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse

#清华源
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```
