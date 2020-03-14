---
layout:     post
title:      "Qt-Creator编译开源项目"
subtitle:   "使用Qt开发CMake开源项目设置教程"
date:       2020-03-13
author:     "Jannon"
header-img: "images/WebHeader/video-card-multimedia-technology-web-header.jpg"
tags:
    - Qt
    - CMake
---

## 下载源码和环境安装
一些开源项目使用的CMake来配置和make来编译安装的，我们需要查看或者修改和调试代码需要在IDE里面进行会方便很多，接下来会说明如何使用QTCreator来开发CMake开源项目KiCad。   
首先需要到KiCad官网下载好源码，根据开源代码的说明配置好开发环境，如开源项目使用说明window平台编译需要用到msys2来安装依赖环境，首先下载安装好msys2之后，运行对应命令环境，注意需要编译多少位的选择对应的位数命令行程序，如果运行的是msys2.exe命令行是没有环境的，使用该命令行会发现安装的命令都找不到，如：  
![camke命令未找到](https://hznhappy.github.io/images/2020/qt-build-cmake/cmakeError.PNG)   
应该运行对应位数的命令行程序：   
![MSYS2-MinGw程序](https://hznhappy.github.io/images/2020/qt-build-cmake/msys64-1.PNG)   
该快捷方式对应的实际程序：   
![MSYS2-MinGW快捷方式](https://hznhappy.github.io/images/2020/qt-build-cmake/msys64-2.PNG)   
打开命令行之后安装项目依赖库环境命令如下：
```
pacman -S base-devel \
          git \
          mingw-w64-x86_64-cmake \
          mingw-w64-x86_64-doxygen \
          mingw-w64-x86_64-gcc \
          mingw-w64-x86_64-python2 \
          mingw-w64-x86_64-pkg-config \
          mingw-w64-x86_64-swig \
          mingw-w64-x86_64-boost \
          mingw-w64-x86_64-cairo \
          mingw-w64-x86_64-glew \
          mingw-w64-x86_64-curl \
          mingw-w64-x86_64-wxPython \
          mingw-w64-x86_64-wxWidgets \
          mingw-w64-x86_64-toolchain \
          mingw-w64-x86_64-glm \
          mingw-w64-x86_64-oce \
          mingw-w64-x86_64-ngspice
cd project-source
mkdir -p build/release
mkdir build/debug               # Optional for debug build.
cd build/release
cmake -DCMAKE_BUILD_TYPE=Release \
      -G "MSYS Makefiles" \
      -DCMAKE_PREFIX_PATH=/mingw64 \
      -DCMAKE_INSTALL_PREFIX=C:/projectName/ \
      -DDEFAULT_INSTALL_PATH=C:/projectName/ \
      ../../
make install

/**
* list all installed packages:
* pacman -Q --explicit
* or pacman -Q -e

* update all local packages:
* pacman -Syu
* (S for sync ,y for refresh,u for sysupgrade)
*/
```
完成以上步骤就可以完成整个开源项目的编译运行环境安装和编译完成整个可运行项目。以上是通过MSYS2命令行方式配置和编译项目，如果需要更好的查看项目代码和调试修改代码，我们需要将项目导入到开发工具IDE当中去。下面就是说明如何使用Qt-Creator来编译项目。

## Qt-Creator配置CMake项目
打开qt-creator，打开项目，选择项目CMakeLists.txt文件，选择构建套件，等待qt生成配置选项，如果报错如下：   
![compiler-failed](https://hznhappy.github.io/images/2020/qt-build-cmake/qt-cmake-build-err.PNG)    

```
Starting to parse CMake project, using: "-DCMAKE_BUILD_TYPE:STRING=Debug", "-DCMAKE_CXX_COMPILER:STRING=C:/Qt/Qt5.13.1/Tools/mingw730_64/bin/g++.exe", "-DCMAKE_C_COMPILER:STRING=C:/Qt/Qt5.13.1/Tools/mingw730_64/bin/gcc.exe", "-DCMAKE_PREFIX_PATH:STRING=D:/msys64/mingw64", "-DQT_QMAKE_EXECUTABLE:STRING=C:/Qt/Qt5.13.1/5.13.1/mingw73_64/bin/qmake.exe".
The C compiler identification is GNU 7.3.0
The CXX compiler identification is GNU 7.3.0
Check for working C compiler: C:/Qt/Qt5.13.1/Tools/mingw730_64/bin/gcc.exe
Check for working C compiler: C:/Qt/Qt5.13.1/Tools/mingw730_64/bin/gcc.exe -- broken
CMake Error at C:/Program Files (x86)/CMake/share/cmake-3.14/Modules/CMakeTestCCompiler.cmake:60 (message):
  The C compiler

    "C:/Qt/Qt5.13.1/Tools/mingw730_64/bin/gcc.exe"

  is not able to compile a simple test program.

  It fails with the following output:
```
qt的编译套件设置的编译器报错，改为之前MSYS2安装的mingw编译器，如下图：   
![compiler错误](https://hznhappy.github.io/images/2020/qt-build-cmake/builder-setting.png)  
如果还是报错如下图所示：   
![compiler-error](https://hznhappy.github.io/images/2020/qt-build-cmake/gcc-error.PNG)  
```
Starting to parse CMake project, using: "-DCMAKE_BUILD_TYPE:STRING=Debug", "-DCMAKE_CXX_COMPILER:STRING=D:\msys64\mingw64\bin\g++.exe", "-DCMAKE_C_COMPILER:STRING=D:\msys64\mingw64\bin\gcc.exe", "-DCMAKE_PREFIX_PATH:STRING=D:/msys64/mingw64", "-DQT_QMAKE_EXECUTABLE:STRING=C:/Qt/Qt5.13.1/5.13.1/mingw73_64/bin/qmake.exe".
The C compiler identification is unknown
CMake Error at C:/Users/Top/AppData/Local/Temp/QtCreator-TkpEOe/qtc-cmake-FcjGRuRJ/CMakeFiles/3.14.0-rc1/CMakeCCompiler.cmake:1 (set):
  Syntax error in cmake code at

    C:/Users/Top/AppData/Local/Temp/QtCreator-TkpEOe/qtc-cmake-FcjGRuRJ/CMakeFiles/3.14.0-rc1/CMakeCCompiler.cmake:1

  when parsing string

    D:\msys64\mingw64\bin\gcc.exe

  Invalid escape sequence \m
Call Stack (most recent call first):
  CMakeLists.txt:33 (project)


Configuring incomplete, errors occurred!
```   
或者报错如下：   
```
CMake Error at C:/msys64/mingw64/share/cmake-3.12/Modules/FindPackageHandleStandardArgs.cmake:137 (message):
  Could NOT find OpenSSL, try to set the path to OpenSSL root folder in the
  system variable OPENSSL_ROOT_DIR (missing: OPENSSL_LIBRARIES) (found
  version "1.0.2p")
Call Stack (most recent call first):
  C:/msys64/mingw64/share/cmake-3.12/Modules/FindPackageHandleStandardArgs.cmake:378 (_FPHSA_FAILURE_MESSAGE)
  CMakeModules/FindOpenSSL.cmake:326 (find_package_handle_standard_args)
  common/CMakeLists.txt:28 (find_package)
```
qt项目设置当中的CMake配置需要设置好DCMAKE_INSTALL_PREFIX字段，告诉CMake依赖库和编译器搜索目录，正确配置修改为：   
![qt-cmake-config](https://hznhappy.github.io/images/2020/qt-build-cmake/msys-makefile-result.png)   

编译过程如果出现以下错误

```
CMake Error at C:/msys64/mingw64/share/cmake-3.12/Modules/FindPackageHandleStandardArgs.cmake:137 (message):
Could NOT find wxWidgets (missing: wxWidgets_LIBRARIES
wxWidgets_INCLUDE_DIRS) (Required is at least version "3.0.0")

```
说明CMake配置设置还是不对，需要修改构建套件当中CMake generator为“MSYS Makefiles”，具体设置如下：   
![CMakeSetting](https://hznhappy.github.io/images/2020/qt-build-cmake/cmake-build-set.png)   

设置完成以上步骤，如果没有报错就能在Qt-Creator当中开发和调试KiCad开源项目了。
