---
layout:     post
title:      "Ubuntu服务器Linux命令"
subtitle:   "Ubuntu系统使用命令记录"
date:       2020-04-02
author:     "Jannon"
header-img: "images/WebHeader/high-performance-computing-server-blue-web-header.jpg"
tags:
    - Ubuntu
    - Command
---

## Ubuntu服务器系统使用Linux命令记录
``` bash
sudo fdisk -l #查看系统存储设备，一般 sdb-(number)如/dev/sdb1表示是U盘
#挂载U盘
sudo mkdir /media/USB
sudo mount -t vfat /dev/sdb1 /media/USB -o [securityoption]
sudo mount -t vfat /dev/sdb1 /media/USB -o uid=1000 #uid=1000用户可以访问
sudo mount -t ntfs-3g /dev/sdb1 /media/USB #NTFS的U盘
sudo umount /dev/sdb1
sudo umount /media/USB

tar -zxvf /home/u/java.tar.gz -C /home/u
mv /home/u/java-se-8u40-ri /home/u/java
sudo sh -c "cat /home/u/javaEnv >> /etc/profile"
source /etc/profile

free -m #以MB为单位显示内存使用情况
free -h #以GB为单位显示内存使用情况
free -s 5 #周期性的查询内存使用信息,每5秒执行一次命令
free -hs 5 #以GB为单位,查询内存使用信息,每5秒执行一次命令

pidof nginx #查看nginx进程的pid
cat /proc/PID/status | grep VmRSS #查看某个pid的物理内存使用情况

pidof java
1973
cat /proc/1973/status | grep VmRSS
VmRSS:   1166852 kB

#查看本机所有进程的内存占比之和
[root@localhost ~]# cat mem_per.sh
#!/bin/bash
ps auxw|awk '{if (NR>1){print $4}}' > /opt/mem_list
awk '{MEM_PER+=$1}END{print MEM_PER}'  /opt/mem_list
[root@localhost ~]#
[root@localhost ~]# chmod 755 mem_per.sh
[root@localhost ~]#
[root@localhost ~]# sh mem_per.sh
64.4

top -d 5 #周期性的查询CPU使用信息，每5秒刷新一次

#查看java进程所占本机的cpu百分比， 如下为0.2%
ps auxw |grep -v grep|grep -w java|awk '{print $3}'
0.2

#查看java进程所占本机的内存百分比， 如下为30.0%
ps auxw |grep -v grep|grep -w java|awk '{print $4}'
30.0

cat /proc/cpuinfo| grep "processor"| wc -l #查看虚拟机逻辑CPU的个数
cat /proc/cpuinfo| grep "physical id"| sort| uniq| wc -l #查看物理CPU个数

lscpu #列出CPU详细信息
cat /proc/cpuinfo| grep "cpu cores"| uniq #查看每个物理CPU中core的个数(即核数)

#获取占用CPU资源最多的10个进程
ps aux | head -1; ps aux | grep -v PID | sort -rn -k +3 | head -10

#获取占用内存资源最多的10个进程
ps aux | head -1; ps aux | grep -v PID | sort -rn -k +4 | head -10

#查看java进程的启动时间和运行时长
ps -ef|grep -v grep|grep -w java|awk '{print $2}'
ps -eo pid,lstart,etime | grep 1973

#查看nginx进程启动的精确时间和启动后运行的时长
ps -eo pid,lstart,etime,cmd|grep nginx

fdisk -l #查看详细的硬盘分区情况
lsblk #查看磁盘情况
df -Th #查看分区、挂载情况
```
