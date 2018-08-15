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
未完待续。。。
