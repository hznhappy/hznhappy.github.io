---
layout:     post
title:      "Terminal/Linux常用命令行"
subtitle:   "开发当中经常使用的命令"
date:       2017-02-16
author:     "Jannon"
header-img: "images/2017/command-line.jpg"
tags:
    - Command Line
---

## Terminal

Terminal是苹果系统自带的命令行终端，使用苹果电脑开发的基本都会接触到。

## ls命令

> ls　　　列出当前目录文件名  
> ls -a　　列出所有文件包括隐藏文件  
> ls -l　　列表形式列出文件和文件的属性  

## cd命令

> cd /  　　　　进入根目录  
> cd    　　　　回到用户目录（用户不同则目录也不同，root为/root，xxx为/Users/xxx  
> cd..  　　　　回到上级目录  
> pwd   　　　　显示当前所在的目录  

## sudo命令

> sudo su 或者sudo –i   　　　切换到root用户  
> exit或者ctr+d         　　　　　　退出root用户  

## 其他命令

> ctrl+A   　　　光标移到到行首  
> ctrl+E   　　　光标移到到行尾  
> ctrl+K   　　　删除命令行光标后面的内容  
> ctrl+w   　　　删除命令行光标前面的内容  
> ctrl+Y   　　　恢复删除的内容  
> ctrl+F   　　　光标向前移动一个字母  
> ctrl+B   　　　光标向后移动一个字母  
> option+← 　光标向前移到一个单词  
> option+→ 　光标向后移到一个单词  
ctrl+c 　　　重新开始一行，编辑模式下会切换到命令模式   
touch name　新建一个指定文件名name的文件  
vi name　　   编辑指定的文件，输入i进入编辑模式，输入：q退出，：wq保存退出，：q!不保存退出


## Xcode快捷键
storyBoard编辑界面快捷键
>cmd+Opt+1..6　　　　　　 　依次对应切换xib文件，帮助，属性，尺寸等观察窗口  
shift+右键/shift+ctrl+左键 　　弹出界面层次菜单选择界面  
option+点击stack建 　　　　　弹出菜单选择unembed取消stackView

代码编辑界面快捷键
>cmd+shift+o　　　　　　快速查找类  
ctrl+6　　　　　　　　　列出文件所有方法  
cmd+F　　　　　　　　　当前文件查找  
cmd+shift+F　　　　　　工程中查找  
cmd+ctrl+F　　　　　　　全屏窗口    
cmd+0　　　　　　　　　隐藏左边导航栏  
cmd+opt+0　　　　　　　隐藏右边导航栏  
cmd+shift+Y　　　　　　隐藏Console调试控制栏  
cmd+opt+enter　　　　　切换成assistant编辑模式  
cmd+enter　　　　　　　　切换回标准编辑模式  
cmd+ctrl+up　　　　　　　切换.h和.m  
cmd+ctrl+left/right　　　　切换当前和上次编辑位置  
cmd+shift+[　　　　　　　切换Tab栏  
ESC 　　　　　　　　　　　代码补全  
cmd+shift+K　　　　　　　清空编译环境  
cmd+.　　　　　　　　　　结束本次调试  
cmd+1　　　　　　　　　　切换成Project导航栏模式

模拟器快捷键
>cmd+1/2/3/4/5 　　　　　　切换显示比例  
opt+shift　　　　　　　　　双指拖动效果  
opt 　　　　　　　　　　　　双指缩放  
cmd+shift+H 　　　　　　　　home键  
cmd+left/right　　　　　　　竖屏切换  

## vim命令
``` shell
/str           查找字符串str，从第一个找 再按小写n下一个匹配，大写N上一个匹配
?str           查找字符串str，从最后一个找，按字母u撤销上次输入
/int\>         匹配单词末尾为int的单词，如print
/\<int         匹配单词开头为int的单词，如interesting
/\<int\>       全单词匹配
set ignorecase 忽略大小写
set noignorecase 恢复大小写敏感
set nu         显示行号
1,100d         删除指定行，删除1-100行
50d            删除第50行
dd             删除光标所在的行
yy             拷贝整行到剪贴板
p              粘贴剪贴板内容
nG             光标跳到第n行
ctrl+G         光标所在的行列打印出来，如光标在第几行第几列
%s/old/new/g   全文件替换
复制操作：
光标移动到起始行：ma          #mark a
光标移动到结束行：mb          #mark b
光标移动到粘贴行：mc          #mark c
:'a,'b, co 'c   把co换成m就是剪切
若要删除多行，则输入：'a,'b de
或者：
：9，15 copy 16  或 ：9，15 co 16
```

## ubuntu挂载手机usb脚本
``` shell
#!/bin/bash
sudo jmtpfs -o allow_other Videos/phone
```

## ubuntu单机双网口光纤网线直连通信设置
``` shell
#!/bin/bash
iptables -t nat -A POSTROUTING -d 192.168.0.100 -s 192.168.2.0/24 -j SNAT --to-source      192.168.2.100
iptables -t nat -A PREROUTING  -d 192.168.0.100 -i xxx           -j DNAT --to-destination 192.168.0.1
iptables -t nat -A POSTROUTING -d 192.168.2.100 -s 192.168.0.0/24 -j SNAT --to-source      192.168.0.100
iptables -t nat -A PREROUTING  -d 192.168.2.100 -i yyy           -j DNAT --to-destination 192.168.2.1

ip route add 192.168.0.100 via 192.168.2.1
ip route add 192.168.2.100 via 192.168.0.1

arp -i xxx -s 192.168.2.100 6c:b3:yy:3c:7b:fe
arp -i yyy -s 192.168.0.100 6c:b3:xx:3c:7b:fc
```
## ubuntu修改分辨率设置
``` shell
#!/bin/bash
xrandr --newmode "1920x1080_60.00"  173.00  1920 2048 2248 2576  1080 1083 1088 1120 -hsync +vsync
xrandr --addmode VGA-1 "1920x1080_60.00"
xrandr --output VGA-1 --mode "1920x1080_60.00"
```

## ubuntu其他一些命令
``` shell
gnome-screensaver-command -l        锁定屏幕

安装qt：
chmod +x qt-opensource-linux-x64-5.11.2.run
./qt-opensource-linux-x64-5.11.2.run

显示设置查看：
sudo lshw -C display
xrandr

qt运行报错找不到库解决：
sudo apt-get install libgl1-mesa-dev
locate libGL.so
sudo ln -s /usr/libx86_64-linux-gnu/mesa/libGL.so.1.7.0 /usr/lib/libGL.so

软链接软件到bin目录
sudo ln -s /home/top/Music/musicplayer /usr/local/bin
```
## ubuntu网络设置相关命令
``` shell
xxx:6c:b3:xx:3c:7b:fc
yyy:6c:b3:yy:3c:7b:fe

sudo ifconfig xxx 10.50.1.11/24
sudo ifconfig yyy 10.50.2.22/24

sudo iptables -t nat -A POSTROUTING -s 10.50.1.11 -d 10.60.2.22 -j SNAT --to-source 10.60.1.11
sudo iptables -t nat -A PREROUTING -d 10.60.1.11 -j DNAT --to-destination 10.50.1.11

sudo iptables -t nat -A POSTROUTING -s 10.50.2.22 -d 10.60.1.11 -j SNAT --to-source 10.60.2.22
sudo iptables -t nat -A PREROUTING -d 10.60.2.22 -j DNAT --to-destination 10.50.2.22

sudo ip route add 10.60.2.22 dev xxx
sudo arp -i xxx -s 10.60.2.22 6c:b3:yy:3c:7b:fe

sudo ip route add 10.60.1.11 dev ens3f1
sudo arp -i yyy -s 10.60.1.11 6c:b3:xx:3c:7b:fc



iptables -t nat -A POSTROUTING -d 192.168.1.100 -s 192.168.2.0/24 -j SNAT --to-source      192.168.2.100
iptables -t nat -A PREROUTING  -d 192.168.1.100 -i eth0           -j DNAT --to-destination 192.168.1.1
iptables -t nat -A POSTROUTING -d 192.168.2.100 -s 192.168.1.0/24 -j SNAT --to-source      192.168.1.100
iptables -t nat -A PREROUTING  -d 192.168.2.100 -i eth1           -j DNAT --to-destination 192.168.2.1


ip route 192.168.1.100 via $ROUTER_2_SUBNET_IP
ip route 192.168.2.100 via $ROUTER_1_SUBNET_IP

echo 1 | sudo tee /proc/sys/net/ipv4/conf/all/proxy_arp
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward


ip netns add ns_server
ip netns add ns_client
ip link set xxx netns ns_server
ip link set yyy netns ns_client
ip netns exec ns_server ip addr add dev xxx 10.50.1.1/24
ip netns exec ns_server ip link set dev xxx up
ip netns exec ns_client ip addr add dev yyy 10.50.1.2/24
ip netns exec ns_client ip link set dev yyy up
ip netns exec ns_server iperf -s -B 10.50.1.1
ip netns exec ns_client iperf -c 10.50.1.1 -B 10.50.1.2
sudo ip netns exec ns_client ip link list
ip netns del ns_server
```

一些使用过的命令记录：
  ``` shell
  #check xxx arp proxy status
  cat /proc/sys/net/ipv4/conf/xxx/proxy_arp

  #Enable xxx arp proxy status
  echo 1 > /proc/sys/net/ipv4/conf/xxx/proxy_arp
  echo 1 > /proc/sys/net/ipv4/ip_forward

  or
  sudo sysctl net.ipv4.conf.xxx.proxy_arp=1
  sudo sysctl -p

  ip route show table local

  sudo tcpdump -n -i eth0 -e arp


  ifconfig xxx 10.50.0.1/24  6c:b3:xx:3c:7b:fc
  ifconfig yyy 10.50.1.1/24  6c:b3:yy:3c:7b:fe

  # nat source IP 10.50.0.1 -> 10.60.0.1 when going to 10.60.1.1
  iptables -t nat -A POSTROUTING -s 10.50.0.1 -d 10.60.1.1 -j SNAT --to-source 10.60.0.1

  # nat inbound 10.60.0.1 -> 10.50.0.1
  iptables -t nat -A PREROUTING -d 10.60.0.1 -j DNAT --to-destination 10.50.0.1

  # nat source IP 10.50.1.1 -> 10.60.1.1 when going to 10.60.0.1
  iptables -t nat -A POSTROUTING -s 10.50.1.1 -d 10.60.0.1 -j SNAT --to-source 10.60.1.1

  # nat inbound 10.60.1.1 -> 10.50.1.1
  iptables -t nat -A PREROUTING -d 10.60.1.1 -j DNAT --to-destination 10.50.1.1


  sudo ip route add 10.60.1.1 dev xxx
  sudo arp -i xxx -s 10.60.1.1 6c:b3:yy:3c:7b:fe # yyy's mac address

  sudo ip route add 10.60.0.1 dev yyy
  sudo arp -i yyy -s 10.60.0.1 6c:b3:xx:3c:7b:fc # xxx's mac address


  ping 10.60.1.1


  # server
  ./iperf -B 10.50.1.1 -s

  # client: your destination is the other end's fake address
  ./iperf -B 10.50.0.1 -c 10.60.1.1 -t 60 -i 10

  tcpdump -nn -i ens3f1 -c 500

  while true ; do egrep 'yyy|xxx' /proc/interrupts ; sleep 1 ; done



  iptables -t nat -L --line-numbers

  iptables -t nat -D PREROUTING 7

  ip route

  netstat -rn = route -n

  ip route show table local

  ip route delete default via <ip> dev enp4s0

  ip route add default via <ip> dev eth0

  ip route delete default via 192.168.2.1 dev enp4s0

  ip route add default via 10.50.1.0 dev ens3f0

  ping -I eth1 google.com

  ping -I eth2 google.com


  ip netns add nc
  ip link add ens0 type veth peer name ens1
  ip link set ens1 netns nc
  ip addr add 10.20.1.1/24 dev ens0
  ip link set dev ens0 up
  ip netns exec nc ip addr add 10.20.1.2/24 dev ens1
  ip netns exec nc ip link set dev ens1 up
  ping 10.20.1.2

  ens3f0:192.168.1.1 gw 192.168.1.1
  ens3f1:192.168.2.1 gw 192.168.2.1

  iptables -t nat -A POSTROUTING -d 192.168.1.100 -s 192.168.2.0/24 -j SNAT --to-source      192.168.2.100
  iptables -t nat -A PREROUTING  -d 192.168.1.100 -i ens3f0           -j DNAT --to-destination 192.168.1.1
  iptables -t nat -A POSTROUTING -d 192.168.2.100 -s 192.168.1.0/24 -j SNAT --to-source      192.168.1.100
  iptables -t nat -A PREROUTING  -d 192.168.2.100 -i ens3f1           -j DNAT --to-destination 192.168.2.1

  ip route add 192.168.1.100 via $ROUTER_2_SUBNET_IP (192.168.2.1)
  ip route add 192.168.2.100 via $ROUTER_1_SUBNET_IP (192.168.1.1)

  arp -i xxx -s 192.168.2.100 6c:b3:yy:3c:7b:fe //ens3f1 mac address
  arp -i yyy -s 192.168.1.100 6c:b3:yy:3c:7b:fc //ens3f0 mac address

  ping 192.168.2.100
  PING 192.168.2.100 (192.168.2.100) 56(84) bytes of data.
  64 bytes from 192.168.2.100: icmp_seq=1 ttl=64 time=0.252 ms
  64 bytes from 192.168.2.100: icmp_seq=2 ttl=64 time=0.201 ms
  64 bytes from 192.168.2.100: icmp_seq=3 ttl=64 time=0.209 ms
  64 bytes from 192.168.2.100: icmp_seq=4 ttl=64 time=0.154 ms
  64 bytes from 192.168.2.100: icmp_seq=5 ttl=64 time=0.164 ms
  ```
