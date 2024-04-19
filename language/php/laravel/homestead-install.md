---
title: "Homestead环境搭建"
date: 2018-10-06T20:40:41+08:00
updateDate: 2018-10-06T20:40:41+08:00
slug: language/php/laravel/homestead-install
image: https://ghjayce.github.io/asset/blog/acN7lrfIQy4GU6zANY9bMjAxODEwMDZfMjA0MDQx.jpeg
categories:
  - 程序设计语言
tags:
  - PHP
  - Laravel
  - Homestead
draft: false
---

环境：
- win7 x64
- powershell v2.0（查看版本 `$PSVersionTable`）

## 搭建Homestead步骤
### 使用Virtualbox
#### 1. 安装 Virtual Box
[Virtual Box 官方下载](https://www.virtualbox.org/wiki/Downloads)

#### 2. 安装 Vagrant
[Vagrant 官方下载](https://www.vagrantup.com/downloads.html)

### 使用VMware
待...

### 3. 在Vagrant中安装
运行下面命令
```bash
$ vagrant box add laravel/homestead
```
选择3 
virtualbox
```bash
==> box: Loading metadata for box 'laravel/homestead'
    box: URL: https://vagrantcloud.com/laravel/homestead
This box can work...

1) hyperv
2) parallels
3) virtualbox
4) vmware_desktop

$ Enter your choice: 3 # step 2
```
> 下载速度慢，可换其他下载器下载
[右键复制链接地址](https://vagrantcloud.com/laravel/boxes/homestead/versions/6.3.0/providers/virtualbox.box)

> 下载好后，运行命令进行安装
```bash
$ vagrant box add laravel/homestead /path/to/virtualbox.box 
```


### 4. 安装Homestead
```bash
$ git clone https://github.com/laravel/homestead.git Homestead
$ cd Homested
```
切换到稳定版本，[homested 发行版](https://github.com/laravel/homestead/releases)
```bash
$ git checkout v7.1.2
$ init.bat
```


### 5. 初始化Homestead
修改文件`homestead.yaml`
```yaml
folders:
    - map: ~/code # 替换为项目路径
```

### 6. 修改hosts
> C:\Windows\System32\drivers\etc\hosts

追加如下内容
```
192.168.10.10 homestead.test
```

### 7. 启动Homestead
```bash
$ vagrant up
```

出现如下错误：
```
The version of powershell currently installed on this host is less than
the required minimum version. Please upgrade the installed version of
powershell to the minimum required version and run the command again.
```

需要更新powershell至5.x
https://docs.microsoft.com/en-us/powershell/wmf/5.1/install-configure

等待初始化完成后，访问 http://homestead.test

> 启动后修改`homestead.yaml`文件需要更新服务

```bash
$ vagrant reload --provision
```
停止服务
```bash
$ vagrant halt
```


## 参考
- [查看手册 - Laravel Homestead 5.6](https://learnku.com/docs/laravel/5.6/homestead/1355)
- [Homestead 2.0 安装笔记 - Summer](https://learnku.com/laravel/t/491/homestead-2-installation-notes)
- [Composer 安装时要求输入授权用户名密码？](https://learnku.com/laravel/t/17893)