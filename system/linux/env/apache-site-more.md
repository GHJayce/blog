---
title: "centos7 安装Apache2.4配置多站点目录"
date: 2018-04-30T10:51:03+08:00
updateDate: 2018-04-30T10:51:03+08:00
slug: system/linux/env/apache-site-more
image: https://ghjayce.github.io/asset/blog/qfHDW8fltBrGpL2TeHDSMjAxODA0MzBfMTA1MTAz.jpeg
categories:
  - 操作系统
tags:
  - Apache
  - Centos
  - Centos7
draft: false
---

## 安装apache
```shell
$ yum install httpd -y
```

### 启动apache
```shell
$ systemctl start httpd.service
```

### 查看是否开启成功
```shell
[root@centos7-1 ~] $ ps -ef|grep httpd
root      1739     1  0 18:34 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache    1740  1739  0 18:34 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache    1741  1739  0 18:34 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache    1742  1739  0 18:34 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache    1743  1739  0 18:34 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache    1744  1739  0 18:34 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
root      1749  1112  0 18:37 pts/0    00:00:00 grep --color=auto httpd
```

### 查看apache端口
```shell
$ netstat -lntup|grep httpd
```

## 修改hosts解析
```shell
$ vi /etc/hosts
```
改成如下内容
```
192.168.56.101 centos7.com  www.centos7.com  bbs.centos7.com  blog.centos7.com
```


### 测试访问
```shell
$ curl www.centos7-1.com
```

## 配置apache

### 备份文件
```shell
$ cp /etc/httpd/conf/httpd.conf /etc/httpd/conf/httpd.conf.back
```

### 配置httpd文件
因为在apache2.4中变化挺大，和nginx一样，可以自定义.conf文件。

在主配置文件中启用虚拟主机
```shell
$ mkdir /etc/httpd/vhost.d/
$ echo "include vhost.d/*.conf"
$ tail -1 /etc/httpd/conf/httpd.conf
```

### 配置多站点目录
```shell
$ vi /etc/httpd/vhost.d/name.conf
```
写入下面的内容
```conf
<VirtualHost *:80>
    ServerAdmin admin@amsilence.com
    DocumentRoot "/var/html/www"
    ServerName www.centos7.com
    ErrorLog "/var/httpd/logs/www-error_log"
    CustomLog "/var/httpd/logs/www-access_log" common
</VirtualHost>

<Directory /var/html/www/>
Require all granted
</Directory>

<VirtualHost *:80>
    ServerAdmin admin@amsilence.com
    DocumentRoot "/var/html/bbs"
    ServerName bbs.centos7.com
    ErrorLog "/var/httpd/logs/bbs-error_log"
    CustomLog "/var/httpd/logs/bbs-access_log" common
</VirtualHost>

<Directory /var/html/bbs/>
Require all granted
</Directory>

<VirtualHost *:80>
    ServerAdmin admin@amsilence.com
    DocumentRoot "/var/html/blog"
    ServerName blog.centos7.com
    ErrorLog "/var/httpd/logs/blog-error_log"
    CustomLog "/var/httpd/logs/blog-access_log" common
</VirtualHost>

<Directory /var/html/blog/>
Require all granted
</Directory>
```

### 重启Apache服务
```shell
$ systemctl restart httpd.service
```

## 测试web访问
```shell
[root@centos7-1 httpd] $ for name in www bbs blog;do curl $name.centos7.com;done;
http://www.centos7.com
http://bbs.centos7.com
http://blog.centos7.com
```

![效果图](https://ghjayce.github.io/asset/blog/d1wuBj2t6MkQ1licaPbTMjAxODA0MzBfMTA1MTAz.png)

## 参考
- [Apache 2.4.6 多域名多网站配置](http://www.linuxidc.com/Linux/2017-04/142621.htm)
- [原文链接：centos7.2 利用yum安装配置apache2.4多虚拟主机](http://blog.csdn.net/wusilen/article/details/54317822)
