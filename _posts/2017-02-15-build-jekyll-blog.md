---
layout:     post
title:      "jekyll+github pages搭建自己的博客"
subtitle:   "github pages+jekyll搭建自己的博客"
date:       2017-02-15
author:     "Jannon"
header-img: "images/2017/bulid-jekyll-blog.jpg"
tags:
    - Jekyll
---

> 这是我的第一篇博客

## 前言  
网上查找资料的时候经常看到一些技术文章博客很好看，其中很大部分都是自己搭建的博客，很多博客都是利用github pages和hexo或者jekyll框架
来搭建自己的博客，于是自己也来动手搭建了一下自己的博客，并且写了这篇博客记录自己的搭建过程。看了[github pages](https://pages.github.com)的说明决定使用jekyll来搭建，搭建首先需要注册一个github账号，如果已经拥有github账号只要建一个[github pages](https://pages.github.com)仓库就可以了，具体的怎么操作参考官网和网上教程。这里就不介绍了。

## 环境准备  
搭建jekyll前你的系统需要准备好[jekyll搭建环境](https://jekyllrb.com/docs/installation/#requirements/)

> GNU/Linux, Unix, 或者 macOS  
> Ruby 2.0或者以上版本  
> GCC 和 Make，你可以在命令行下输入gcc -v 和 make -v查看是否安装   

由于我是使用mac OS X操作系统，并且安装了Xcode开发工具，因此很多环境都已经自带了，因此不需要安装准备任何环境，只需要很简单的命令行就可以完成jekyll环境的安装。

## 搭建jekyll步骤  
[Jekyll官网](https://jekyllrb.com/docs/quickstart/)有具体的快速搭建jekyll的向导，具体搭建步骤是通过RubyGems安装Jekyll 和 Bundler gems,建立jekyll站点文件夹myblog, 进入你刚刚创建的文件夹启动建立的jekyll站点服务。该过程在terminal或者其他命令行工具中命令如下：

> ~ $ gem install jekyll bundler  
> ~ $ jekyll new myblog  
> ~ $ cd myblog  
> ~/myblog $ bundle exec jekyll serve

现在你可以通过 http://localhost:4000 看到jekyll已经搭建好了的默认页面

jekyll的各个目录结构，具体作用以及如何写博客都可以参考[jekyll官网](https://jekyllrb.com)说明或者网上一大堆的教程。

## 发布博客到gihub pages

新建的jekyll博客只是在本地，自己可以访问，但是要别人也可以访问你的博客就需要把本地建立好的博客部署到gihub pages仓库中，部署命令如下：

> cd 你的博客目录  
> git init  
> git add .  
> git commit -m "publish blog"  
> git remote add origin https://github.com/yourgihubname/yourgithubname.github.io.git  
> git push -u origin master  

现在别人可以通过https://yourgithubname.github.io打开并查看你的博客了。
