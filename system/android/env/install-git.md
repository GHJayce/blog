---
title: "在android上安装git环境"
slug: system/android/env/install-git
date: 2023-08-30T13:26:26+08:00
categories:
    - 操作系统
tags:
    - android
    - git
    - 环境安装
    - termux
---
## 前言
一开始打算在手机上安装git，想到的是安装app，于是各种找，找到了有如：
1. [agit](https://github.com/rtyley/agit)
2. [MGit](https://github.com/maks/MGit)
3. ...

但是试过这些app以后发现要么BUG很多，要么根本不能正常使用，而且项目也很久没有更新了，只能另外再找别的app。

机缘巧合下，让我找到了口袋git（com.aor.pocketgit）的app，正常使用了一段时间，最近再用的时候，频繁出现了`Failed to fetch my_project.`，没有办法fetch项目，也就没法继续使用了。

开始我以为是账号密码的认证方式出了问题，于是我尝试换另一种认证方式——私钥，但是它的私钥是`.ppk`的格式，和ssh生成的密钥不太一样，一轮搜索以后：
- 说是需要用到`PuTTy`的`PuTTY Key Generator`来对ssh的密码进行转换处理得到`.ppk`的文件。
- 按照步骤转换好以后再放到app上，显示了感叹号，感觉不太对劲，进行fetch操作时果然，提示`Invalid Private Key.`

好家伙，ppk的方式也不行，也不想再试了，那为什么用的好好的账密认证方式出了问题呢？

我估计是最近github强制使用2FA的关系，登录账号需要经过两步验证，app应该是没有做这个异常处理，而且在app内也没有办法升级。

于是我放弃了这个app，换了另一条路。

## 安装git环境
在逛Stack Overflow时发现了[Termux](https://github.com/termux/termux-app)这个app，它是在Android上运行的一个终端软件，也就是直接敲命令在手机上安装git环境。

打开app，执行以下命令安装：
```bash
$ apt update
$ apt install git
```
安装完以后，熟悉的操作方式就回来了：
```bash
$ git --version
git version 2.42.0
```

剩下的就不用多说了，ssh-keygen走一波，下面贴一下我遇到的问题和解决过程。
### 操作过程
第一时间就是进入到项目目录，先看看自己在哪个目录：
```bash
$ pwd
/data/data/com.termux/files/home
# 然后查看根目录都有些什么
$ ls /
ls: cannot open directory '/': Permission denied
```
竟然没权限，`whoami`一看，哦，看来还是root用习惯了，普通用户只拥有`/data/data/com.termux`目录下的权限，而`/data/data`之前的目录都没有权限。

那怎么搞？没有权限我怎么知道我的项目在哪个路径下？？

熟悉安卓开发的同学或者搞过机的同学就知道，我们在文件管理看到的根目录，是在`/storage/emulated/0/`下，我也是在折腾了一番以后才得知。

这么长的路径，不行，我得设置成全局变量才行
```bash
# 习惯用vim去编辑，其实还有别的方式
$ vim ~/.bash_profile
The program vim is not installed. Install it by executing:
pkg install vim
or
pkg install vim-gtk, after running pkg in x11-repo
or
pkg install vim-python
```
这里我执行了`pkg install vim`，然后你懂的。

然后cd进项目路径，查看配置信息：
```bash
$ git config --list
fatal: detected dubious ownership in repository at '/xxx/xxx' To add an exception for this directory, call:
git config --global --add safe.directory /xxx/xxx
```
跟着提示将我们的项目路径设置成安全目录就行。

```bash
$ ssh-keygen -t rsa -C "your@email.com"
The program ssh-keygen is not installed. Install it by executing:
pkg install openssh
```
跟着提示执行`pkg install openssh`，然后遇到连续四个`CANNOT LINK EXECUTABLE "ssh-keygen": library "libcrypto.so.3" not found`。

这里需要执行`pkg install openssl`安装相关依赖，再重新执行安装命令就可以了。

```bash
$ git fetch origin
CANNOT LINK EXECUTABLE "/data/data/com.termux/files/usr/libexec/git-core/git-remote-https": library "libssl.so.1.1" not found
```
解决：
```bash
$ find /data/data/com.termux/files -name 'libssl.so.*'
/data/data/com.termux/files/usr/lib/openssl-1.1/libssl.so.1.1
/data/data/com.termux/files/usr/lib/libssl.so.3
# 如果没有的话，先执行
pkg install openssl1.1-tool
# 添加环境变量
echo "export LD_LIBRARY_PATH=/data/data/com.termux/files/usr/lib/openssl-1.1" >> ~/.bash_profile && source ~/.bash_profile
```
新的问题：
```bash
$ git fetch origin
fatal unable to access 'https://github.com/xxx.git': HTTP/2 stream 1 was not closed cleanly before end of the underlying stream
# 解决：
$ git config --global http.version HTTP/1.1
```

## 参考
1. [How to use Git on Android? - Stack Overflow](https://stackoverflow.com/questions/2701078/how-to-use-git-on-android)
2. [生成Git ssh公钥和私钥（ppk）文件](https://www.rstk.cn/news/1474415.html)
3. [Termux-setup-storage - Termux Wiki](https://wiki.termux.com/wiki/Termux-setup-storage)
4. [library “libssl.so.1.1“ not found 安卓神器termux使用命令时的报错。\_4ustn1ne的博客-CSDN博客](https://blog.csdn.net/qq_42560204/article/details/125670804)
