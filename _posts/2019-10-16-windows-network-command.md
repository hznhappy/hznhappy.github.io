---
layout:     post
title:      "Window网络命令"
subtitle:   "Window系统命令行网络命令"
date:       2019-10-16
author:     "Jannon"
header-img: "images/about-bg.jpg"
tags:
    - window
    - Command Line
---

## 查看系统连接过的wifi设置
如电脑连接过的wifi密码忘记了，可以通过命令行查看系统连接过的wifi信息，如密码，并可以以xml文件导出，具体过程和命令如下：   
查看电脑连接过的wifi配置文件：
``` shell
netsh wlan show profiles
```
命令输出结果如下：
``` shell

接口 WLAN 上的配置文件:
策略配置文件(只读)
---------------------------------
    <无>
用户配置文件
-------------
    所有用户配置文件 : PekingUniversity
```

查看wifi配置信息命令如下：
``` shell
netsh wlan show profile name="PekingUniversity" key=clear

在计算机上找不到配置文件“PekingUniversity”。    

```

如果出现找不到配置文件，使用管理员命令行进行重新输入命令即可，结果如下：
``` shell
netsh wlan show profile name="PekingUniversity" key=clear

接口 WLAN 上的配置文件 PekingUniversity:
=======================================================================
已应用: 所有用户配置文件
配置文件信息
-------------------
    版本                   : 1
    类型                   : 无线局域网
    名称                   : PekingUniversity
    控制选项               :
        连接模式           : 自动连接
        网络广播           : 只在网络广播时连接
        AutoSwitch         : 请勿切换到其他网络
        MAC 随机化: 禁用
连接设置
---------------------
   SSID 数目              : 1
    SSID 名称              :“PekingUniversity”
    网络类型               : 结构
    无线电类型             : [ 任何无线电类型 ]
    供应商扩展名           : 不存在
安全设置
-----------------
    身份验证         : WPA2 - 个人
    密码                 : CCMP
    身份验证         : WPA2 - 个人
    密码                 : 未知
    安全密钥               : 存在
    关键内容            : php951753
费用设置
-------------
    费用                : 无限制
    阻塞                : 否
    接近数据限制        : 否
    过量数据限制        : 否
    漫游                : 否
    费用来源            : 默认
```
可以看到安全设置部分关键内容即是wifi密码信息。   
还可以将设置导出为文件保存：
``` shell
netsh wlan export profile name="PekingUniversity" folder=. key=clear
接口配置文件“PekingUniversity”已成功保存在文件“.\WLAN-PekingUniversity.xml”中。
```
folder=.表示保存到当前目录。文件信息如下：
``` html
<?xml version="1.0"?>
<WLANProfile xmlns="http://www.microsoft.com/networking/WLAN/profile/v1">
	<name>PekingUniversity</name>
	<SSIDConfig>
		<SSID>
			<hex>50656B696E67556E6976657273697479</hex>
			<name>PekingUniversity</name>
		</SSID>
	</SSIDConfig>
	<connectionType>ESS</connectionType>
	<connectionMode>auto</connectionMode>
	<MSM>
		<security>
			<authEncryption>
				<authentication>WPA2PSK</authentication>
				<encryption>AES</encryption>
				<useOneX>false</useOneX>
			</authEncryption>
			<sharedKey>
				<keyType>passPhrase</keyType>
				<protected>false</protected>
				<keyMaterial>php951753</keyMaterial>
			</sharedKey>
		</security>
	</MSM>
	<MacRandomization xmlns="http://www.microsoft.com/networking/WLAN/profile/v3">
		<enableRandomization>false</enableRandomization>
		<randomizationSeed>3674483165</randomizationSeed>
	</MacRandomization>
</WLANProfile>
```
可以看到keyMaterial字段即是wifi密码。

一些其他命令：
``` shell
netsh wlan show networks mode=bssid  #列出所有可连接wifi信息
netsh wlan delete profile name="wifiname" #删除wifi配置文件
netsh wlan add profile filename="wifi.xml" #默认当前文件夹
netsh wlan connect name="wifiname" #连接wifi
netsh interface set interface "Interface Name" enabled #启用接口
netsh wlan show interface
系统上有 1 个接口:
    名称                   : WLAN
    描述                   : Intel(R) Dual Band Wireless-AC 3165
    GUID                   : 1d6831cb-0b2d-4703-ba79-26f19f25c0ad
    物理地址               : 64:5d:86:49:01:29
    状态                   : 已断开连接
    无线电状态           : 硬件 开
                             软件 开
    承载网络状态  : 不可用
```

## 设置电脑的无线网卡为wifi
如果电脑有无线网卡，可以通过命令设置无线网卡为wifi，可以给其他设备连接该电脑wifi的网络联网。   
首先查看是否支持设置为wifi：
``` shell
netsh wlan show drivers

接口名称: WLAN
    驱动程序                  : Intel(R) Dual Band Wireless-AC 3165
    供应商                    : Intel Corporation
    提供程序                  : Intel
    日期                      : 2018/2/25
    版本                      : 20.40.0.100
    INF 文件                  : 慤摳慳㈱
    类型                      : 本机 WLAN 驱动程序
    支持的无线电类型          : 802.11b 802.11g 802.11n 802.11a 802.11ac
    支持 FIPS 140-2 模式: 是
    支持 802.11w 管理帧保护 : 是
    支持的承载网络  : 否
```

支持的承载网络  : 否-----表示不支持，这种情况命令行设置不了wifi，但是可以安装一个360或者猎豹无线wifi也可以设置成wifi使用，但是本人测试不是很稳定，经常会自动断开。如果是支持的承载网络可以设置如下：
``` shell
netsh wlan set hostednetwork mode=allow ssid=tests key=tests123

netsh wlan start hostednetwork

netsh wlan show hostednetwork
```

## 查看局域网中能ping通的主机
``` shell
for /L %i IN (1,1,254) DO ping -w 2 -n 1 192.168.1.%i | grep TTL >> a.txt
arp -a
#查看可以通过TCP/IP连接4370端口的ip地址
for /L %i IN (1,1,255) DO telnet 192.168.1.%i 4370| grep Connected >> tcp.txt
```
