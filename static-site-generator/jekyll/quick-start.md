---
title: "本地快速搭建Jekyll及相关依赖"
date: 2018-01-02T03:39:35+08:00
updateDate: 2018-01-02T03:39:35+08:00
slug: static-site-generator/jekyll/quick-start
image: https://ghjayce.github.io/asset/blog/l2X2IUWhKLBkgz5kr7qOMjAxODAxMDJfMDMzOTM1.jpeg
categories:
  - 工具
tags:
  - Jekyll
  - 静态站点生成器
  - 教程
draft: false
---

文章为过程记录

> 另外，以下命令行相关的全局变量配置，需要自己去设置。例如：ruby、python

相关链接：[Jekyll教程——精心收藏](https://www.cnblogs.com/paxster/p/3917639.html)

参考教程：[Julian Thilo写的不错的安装教程](http://jekyll-windows.juthilo.com/) **一定要看**

[官方教程](https://jekyllrb.com/docs/home/)

## 环境
- windows7 x64
- ruby 2.4.2p198 x64
- python 2.7.13rc1

## 下载安装ruby相关

> Ruby是Jekyll编写的编程语言。您需要安装Ruby和相应的DevKit

虽然我已经有ruby的环境，但是也记录一下

[下载地址](https://rubyinstaller.org/downloads/)

按需所取，我选的如下

![图片描述](https://ghjayce.github.io/asset/blog/FNrhe5HA34CW3D49A87DMjAxODAxMDJfMDAwMDAw.jpeg)


### 安装Ruby DevKit

将下载好的`ruby devkit`选择`C:\RubyDevKit\`解压存档

运行命令行工具（cmd），依次执行下面的代码
```
cd C:\RubyDevKit    # step1
ruby dk.rb init     # step2
ruby dk.rb install  # step3
```


### 安装Ruby Gem

> Jekyll本身就是Ruby Gem的形式，它是一个易于安装的软件包。

紧接着执行
```
gem install jekyll # step4
```

### 安装rouge

> 无论您使用的是Markdown还是HTML，Jekyll都可以让您轻松地将精美的代码块插入到您的网页中。

语法高亮，快速和无痛
```
gem install rouge # step5
```

然后，在你的中`_config.yml`，将Rouge设置为你的语法高亮部分：
```
highlighter: rouge
```

## 安装python相关

> 如果你想使用Pygments，这是一个默认的Jekyll依赖，在Windows上的语法突出显示，你需要安装Python，PIP，最后是pygments.rb的Python基础。

教程文章说要装2.7的版本，3的版本可能无法正常工作。

![图片描述](https://ghjayce.github.io/asset/blog/S5xmwpkIhycfsJwzcqKlMjAxODAxMDJfMDAwMDAw.webp)

### 安装pip

> Pip是一个安装和管理Python包的工具，类似于Ruby Gems。

浏览器打开：https://bootstrap.pypa.io/get-pip.py

右键保存到`C:\pip\`

然后打开命令行工具（cmd），并依次执行
```
cd C:\pip                       # step6
python get-pip.py               # step7
python -m pip install Pygments  # step8
```

> 设置Pygments作为你的语法突出显示

如果尚未设置，请添加以下内容_config.yml，将Pygments设置为语法突出显示。
```
highlighter: pygments
```


## jekyll watch

> Jekyll有一个内置的自动更新功能，监视您的源文件夹的变化，然后重新建立您的网站。

命令行工具（cmd），执行下面步骤

> 在Windows上，您需要安装一个额外的工具，或者Gem，以启用此功能。

```
gem install wdm # step9
```

## 创建一个项目，并运行它

打开命令行工具（cmd），执行下面的步骤
```
gem install jekyll bundler # 先安装这个 step10
# 可以查看jekll版本了
# jekyll -v
cd desktop  # 一打开应该是自己的用户目录，直接可以cd到桌面 step11
jekyll new my_blog # step12
cd my_blog # step13
jekyll server --watch #step14
```

在浏览器访问`http://localhost:4000/`搭建好的环境，并且是实时更新的！

![图片描述](https://ghjayce.github.io/asset/blog/rICprhx6nhgadT8O2e5IMjAxODAxMDJfMDAwMDAw.webp)
