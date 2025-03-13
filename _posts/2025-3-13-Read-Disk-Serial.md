---
layout:     post
title:      "使用接口获取硬盘序列号"
subtitle:   "Linux系统获取硬盘序列号"
date:       2025-02-20
author:     "Jannon"
header-img: "images/WebHeader/computer-technology-business-website-header.jpg"
tags:
	- SVN
---

## 方法一：使用udev库接口

```c
#include <libudev.h>
#include <stdio.h>
#include <sys/stat.h>
#include <sys/types.h>

int main() {
    struct udev *udev;
    struct udev_device *dev, *parent_dev;
    struct stat root_stat;

    // 获取根目录的设备信息
    if (stat("/", &root_stat) != 0) {
        perror("无法获取根目录信息");
        return 1;
    }

    // 创建 udev 对象
    udev = udev_new();
    if (!udev) {
        fprintf(stderr, "无法创建 udev 对象\n");
        return 1;
    }

    // 获取根目录对应的设备
    dev = udev_device_new_from_devnum(udev, 'b', root_stat.st_dev);
    if (!dev) {
        fprintf(stderr, "无法获取设备信息\n");
        udev_unref(udev);
        return 1;
    }

    // 获取父设备（硬盘）
    parent_dev = udev_device_get_parent_with_subsystem_devtype(dev, "block", NULL);
    if (!parent_dev) {
        fprintf(stderr, "无法获取父设备信息\n");
        udev_device_unref(dev);
        udev_unref(udev);
        return 1;
    }

    // 获取硬盘的序列号
    const char *serial = udev_device_get_property_value(parent_dev, "ID_SERIAL_SHORT");
    if (serial) {
        printf("根目录所在硬盘的序列号: %s\n", serial);
    } else {
        printf("未找到硬盘序列号\n");
    }

    // 释放资源
    udev_device_unref(dev);
    udev_unref(udev);

    return 0;
}
```

打印系统中所有硬盘序列号：

```c
#include <libudev.h>
#include <stdio.h>

int main() {
    struct udev *udev;
    struct udev_enumerate *enumerate;
    struct udev_list_entry *devices, *dev_list_entry;
    struct udev_device *dev;

    // 创建 udev 对象
    udev = udev_new();
    if (!udev) {
        fprintf(stderr, "无法创建 udev 对象\n");
        return 1;
    }

    // 枚举块设备
    enumerate = udev_enumerate_new(udev);
    udev_enumerate_add_match_subsystem(enumerate, "block");
    udev_enumerate_scan_devices(enumerate);
    devices = udev_enumerate_get_list_entry(enumerate);

    // 遍历设备
    udev_list_entry_foreach(dev_list_entry, devices) {
        const char *path = udev_list_entry_get_name(dev_list_entry);
        dev = udev_device_new_from_syspath(udev, path);

        // 获取设备名称（如 sda）
        const char *devname = udev_device_get_devnode(dev);
        if (devname) {
            // 获取序列号
            const char *serial = udev_device_get_property_value(dev, "ID_SERIAL");
            if (serial) {
                printf("设备: %s, 序列号: %s\n", devname, serial);
            }
        }

        udev_device_unref(dev);
    }

    // 释放资源
    udev_enumerate_unref(enumerate);
    udev_unref(udev);

    return 0;
}
```



## 方法二：使用ioctl接口

```c
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <unistd.h>
#include <sys/ioctl.h>
#include <linux/hdreg.h>

int main() {
    const char *device = "/dev/nvme1n1p2"; // 设备路径
    int fd;
    struct hd_driveid id;

    // 打开设备
    fd = open(device, O_RDONLY);
    if (fd < 0) {
        perror("Failed to open device");
        return 1;
    }

    // 使用 ioctl 获取硬盘信息
    if (ioctl(fd, HDIO_GET_IDENTITY, &id) < 0) {
        perror("Failed to get drive identity");
        close(fd);
        return 1;
    }

    // 打印序列号
    printf("Serial number: %.20s\n", id.serial_no);

    // 关闭设备
    close(fd);
    return 0;
}

```

## 方法三：通过文件

```c
#include <stdio.h>
#include <mntent.h>
#include <sys/ioctl.h>
#include <stdlib.h>
#include <limits.h>
#include <unistd.h>
#include <fcntl.h>
#include <linux/hdreg.h>
#include <string>
#include <cstring>
using namespace std;
int main() {
    struct mntent *ent;
    FILE *fp = setmntent("/proc/mounts", "r");
    if (!fp) {
        perror("Failed to open /proc/mounts");
        return 1;
    }
    string dev_path;
    while ((ent = getmntent(fp)) != NULL) {
        if (strcmp(ent->mnt_dir, "/") == 0) {
            printf("根目录挂载设备：%s\n", ent->mnt_fsname);
            dev_path = ent->mnt_fsname;
            break;
        }
    }
    printf("----%s\n",dev_path.c_str());
    endmntent(fp);

     static char parent[256];
    char sysfs_path[PATH_MAX];
    char real_path[PATH_MAX];

    // 提取分区名（如 "/dev/sda1" → "sda1"）
    const char *slash = strrchr(dev_path.c_str(), '/');
    if (!slash) return NULL;
    const char *devname = slash + 1;

    // 构造 sysfs 路径（如 /sys/class/block/sda1/..）
    snprintf(sysfs_path, sizeof(sysfs_path), "/sys/class/block/%s/..", devname);

    // 解析父设备的 sysfs 路径
    if (realpath(sysfs_path, real_path) == NULL) return NULL;

    // 提取父设备名（如 /sys/devices/pci0000:00/.../sda → sda）
    const char *parent_name = strrchr(real_path, '/');
    if (!parent_name) return NULL;
    parent_name++;

    snprintf(parent, sizeof(parent), "/dev/%s", parent_name);
    printf("-----%s\n",parent);


    //先找一下文件/sys/block/slash/devices/serial
    string serialFile  = "/sys/block/";
           serialFile += parent_name;
           serialFile += "/device/serial";
    printf("序列号文件:%s\n",serialFile.c_str());
    int Fd;
    char buffer[256];
    ssize_t bytes_read;

    // 打开文件
    Fd = open(serialFile.c_str(), O_RDONLY);
    if (Fd == -1) {
        perror("打开文件失败");
    }

    // 读取内容
    bytes_read = read(Fd, buffer, sizeof(buffer) - 1);
    if (bytes_read == -1) {
        perror("读取文件失败");
        close(Fd);
    }
    else
    {
        // 处理数据
        buffer[bytes_read] = '\0'; // 添加字符串终止符
        printf("设备序列号: %s", buffer);
        close(Fd);
    }
	return 0;
}
```

