---
layout:     post
title:      "设置本地Jekyll网站调试GithubPages"
subtitle:   "Setting up your GitHub Pages site locally with Jekyll"
date:       2018-08-05
author:     "Jannon"
header-img: "images/about-bg.jpg"
tags:
    - blog
---

## 前言
我们在github上建立好了个人的博客仓库githubPages之后，通常需要写博客的时候会在本地克隆一个本地的githubPage仓库进行博客的写作，然后push到远程的githubPage仓库。如果我们需要本地调试自己的博客的时候，可以通过工具查看自己的当前所写博客效果，如Atom。但是如果要看到博客发布后各种链接跳转效
果就需要通过git push到github才能查看效果比较麻烦。因此我们可以通过jekyll设置好githubPages本地网站，通过本地进行最终效果的调试，然后在合并到github。以下内容是比较简单的介绍了自己建立过程用到的步骤，实际是每个人环境不同可能需要的步骤更多，根据自己的需求还需要哪些步骤可以看看[官方文档](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)很详细的介绍了各种步骤。

## 环境准备
首先需要安装好jekyll环境，具体参考我之前的[博客链接](https://hznhappy.github.io/2017/02/15/build-jekyll-blog/),
然后clone你的githubPages仓库到本地。

## 用bundle给githubPages本地仓库创建jekyll
Ruby会用Gemfile里面的内容来建立你的jekyll网站和环境依赖。首先切换到你的gitHubPages本地仓库目录中。

1.在gihubPages仓库中添加一个GemFile文件，文件内容如下：
```
source "https://ruby.taobao.org/"
gem 'github-pages', group: :jekyll_plugins
```

2.如果你的仓库里面有这个文件，就编辑该文件，加入以上内容，其他的内容可以注释掉，例如jekyll网站建立的默认Gemfile文件的一些内容,注释和保留内容如下：
```
#gem "jekyll", "~> 3.8.3"

# This is the default theme for new Jekyll sites. You may change this to anything you like.
#gem "minima", "~> 2.0"
#gem 'jekyll-sitemap'
#gem 'jekyll-paginate'
gem "github-pages", group: :jekyll_plugins

# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.6"
end
```

3.通过bundle从githubPages gem安装jekll和环境依赖
```
$bundle install
Fetching gem metadata from https://ruby.taobao.org/............
Fetching gem metadata from https://ruby.taobao.org/..
Resolving dependencies.....
```

## 运行本地gitHubPages的Jekyll网站
1.切换到你的githubPages本地仓库文件夹

2.运行jekyll本地网站
```
$ bundle exec jekyll serve
Configuration file: /Users/UserName/Documents/Blog/_config.yml
       Deprecation: The 'gems' configuration option has been renamed to 'plugins'. Please update your config file accordingly.
            Source: /Users/UserName/Documents/Blog
       Destination: /Users/UserName/Documents/Blog/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 2.279 seconds.
 Auto-regeneration: enabled for '/Users/UserName/Documents/Blog'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
  ```

3.通过编写blog文件可以在本地实时查看和调试最终效果了。

## 更新自己本地网站
jekyll和Github Pages 服务器会经常更新，因此也需要通过以下命令更新自己本地的环境：
> bundle update github-pages

或者通过以下命令更新自己环境下的所有gem：
> bundle update

如果你没有安装bundle，可以通过以下命令更新：
> gem update github-pages
