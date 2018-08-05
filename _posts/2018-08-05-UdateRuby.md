---
layout:     post
title:      "升级Ruby版本"
subtitle:   "RVM升级Ruby"
date:       2018-08-05
author:     "Jannon"
header-img: "images/about-bg.jpg"
tags:
    - Ruby
---

## 安装RVM
RVM：Ruby Version Manager，Ruby版本管理器，可以管理ruby和gem版本。  

命令行依次输入以下命令安装RVM：
``` bash
$ curl -L get.rvm.io | bash -s stable
$ source ~/.bashrc  
$ source ~/.bash_profile
```    
安装完成后，查看版本号，看是否安装成功：  
``` bash
$ rvm -v
rvm 1.29.4 (latest) by Michal Papis, Piotr Kuczynski, Wayne E. Seguin [https://rvm.io]
```
 ## 升级Ruby
 1.查看rvm可升级ruby版本列表：
 ``` bash
 $ rvm list known
# MRI Rubies
[ruby-]1.8.6[-p420]
[ruby-]1.8.7[-head] # security released on head
[ruby-]1.9.1[-p431]
[ruby-]1.9.2[-p330]
[ruby-]1.9.3[-p551]
[ruby-]2.0.0[-p648]
[ruby-]2.1[.10]
[ruby-]2.2[.10]
[ruby-]2.3[.7]
[ruby-]2.4[.4]
[ruby-]2.5[.1]
[ruby-]2.6[.0-preview2]
ruby-head
```
2.安装指定版本的ruby
```
$ rvm install 2.5
```
3.安装完成，查看ruby版本
```
$ ruby -v
ruby 2.5.1p57 (2018-03-29 revision 63029) [x86_64-darwin16]
```
至此，通过rvm安装升级ruby完成，可以看到rvm和ruby安装目录如下：
```
$ which rvm
/Users/username/.rvm/bin/rvm

$ which ruby
/Users/username/.rvm/rubies/ruby-2.5.1/bin/ruby
```
