---
layout:     post
title:      "Ubuntu12.04安装qt开发环境"
subtitle:   "32位ubuntu安装32位qt"
date:       2019-10-11
author:     "Jannon"
header-img: "images/homepage-bg.jpg"
tags:
    - QT
    - ubuntu
---
## 软件
系统：ubuntu-12.04.5(32bit)    [下载地址](https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/12.04/ubuntu-12.04.5-desktop-i386.iso)   
qt：qt-everywhere-opensource-src-4.7.4.tar.gz   [下载地址](https://mirrors.tuna.tsinghua.edu.cn/qt/archive/qt/4.7/qt-everywhere-opensource-src-4.7.4.tar.gz)   
qtCreator：qt-creator-linux-x86-opensource-2.4.1.bin [下载地址](https://mirrors.tuna.tsinghua.edu.cn/qt/archive/qtcreator/2.4/qt-creator-2.4.1-src.tar.gz)

## 安装qt

``` shell
sudo apt-get install g++
sudo apt-get install libx11-dev libxext-dev libxtst-dev
tar xzvf qt-everywhere-opensource-src-4.7.4.tar.gz
cd qt-everywhere-opensource-src-4.7.4.tar.gz
./configure
```
此时终端输入o回车选择开源版本，接着输入yes同意安装协议。   
配置完成之后：
``` shell
Qt is now configured for building. Just run 'make'.
Once everything is built, you must run 'make install'.
Qt will be installed into /usr/local/Trolltech/Qt-4.8.6

To reconfigure, run 'make confclean' and 'configure'.
```
根据提示输入make进行编译，等待漫长的时间，编译完成输入make install完成qt环境的安装。

本人编译过程遇到了报错，编译（webkit）模块的时候出现："cannot find -lXrender" 
网上找到的解决办法如下：
```
确认安装了libX11-dev 

如果还不行: 

cd /usr/lib 

ln -s libXrender.so.1 libXrender.so 

(其中的libXrender.so.1为安装的x11版本） 

注意： 编译时需要修改mkspecs/xxx文件夹的qmake.conf默认X11和OpenGL配置 

默认配置-> 

QMAKE_LIBDIR_X11 = /usr/X11R6/lib64 
QMAKE_LIBDIR_OPENGL = /usr/X11R6/lib64 

例：（ubuntu 10.10 X64） 

QMAKE_LIBDIR_X11 = /usr/lib/X11
```
这个方法不会用没有成功，贴出来给大家一个参考。具体怎么解决由于时间关系，没有深入去解决这个问题，由于本人没有用到webkit模块，所以去掉了这个模块，重新编译:
``` shell
make   uninstall
make   clean
make   confclean
./configure -no-webkit
make  (这个编译时间非常长)
make install
```
所以如果没有用到webkit模块可以一开始就./configure -no-webkit进行配置，防止后面报错重新配置。

## 设置环境变量
安装完成qt之后，设置系统环境变量：
``` shell
sudo gedit /etc/profile
```
在该文件末尾添加如下内容：
```
export QTDIR=/usr/local/Trolltech/Qt-4.7.4
export PATH=$QTDIR/bin:$PATH
export MANPATH=$QTDIR/man:$MANPATH
export LD_LIBRARY_PATH=$QTDIR/lib:$LD_LIBRARY_PATH
```

``` shell
source /etc/profile
```
使变量生效。

## 安装qt creator
``` shell
chmod +x qt-creator-linux-x86-opensource-2.4.1.bin
./qt-creator-linux-x86-opensource-2.4.1.bin
```
根据安装向导完成qt开发工具qt creator的安装，至此安装完毕，可以开始qt编程开发。

## Ubuntu18.04系统qt安装
Ubuntu18.04系统安装qt5.11.2比较简单，下载完成qt软件`qt-opensource-linux-x64-5.11.2.run`之后，按步骤执行以下命令：
``` bash
./qt-opensource-linux-x64-5.10.1.run #安装到用户目录
#或者安装到系统/opt目录
sudo ./qt-opensource-linux-x64-5.10.1.run

sudo apt-get update
sudo apt-get upgrade
sudo apt-get install build-essential
sudo apt-get install gcc g++ cmake
#运行qtcreator如果报错cannot find -lGL“ =可选安装：“libglu1-mesa-dev”
sudo apt-get install libgl1-mesa-dev
#如果还报错cannot find -lGL
$ locate libGL.so
/snap/gnome-3-26-1604/70/usr/lib/x86_64-linux-gnu/mesa/libGL.so.1
/snap/gnome-3-26-1604/70/usr/lib/x86_64-linux-gnu/mesa/libGL.so.1.2.0
/snap/gnome-3-26-1604/98/usr/lib/x86_64-linux-gnu/mesa/libGL.so.1
/snap/gnome-3-26-1604/98/usr/lib/x86_64-linux-gnu/mesa/libGL.so.1.2.0
/snap/gnome-3-28-1804/116/usr/lib/x86_64-linux-gnu/libGL.so.1
/snap/gnome-3-28-1804/116/usr/lib/x86_64-linux-gnu/libGL.so.1.0.0
/usr/lib/x86_64-linux-gnu/libGL.so.1
/usr/lib/x86_64-linux-gnu/libGL.so.1.0.0
$ sudo ln -s /usr/lib/x86_64-linux-gnu/libGL.so.1 /usr/lib/libGL.so


#编译如果报错error: GL/gl.h: No such file or directory
sudo apt-get install mesa-common-dev
```
