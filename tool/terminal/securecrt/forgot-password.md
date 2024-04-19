---
title: "使用python找回在SecureCRT中的Linux登入密码"
date: 2018-03-17T12:00:50+08:00
updateDate: 2018-03-17T12:00:50+08:00
slug: tool/terminal/securecrt/forgot-password
image: https://ghjayce.github.io/asset/blog/oQBWE3gpg8IKTskSpyzrMjAxODAzMTdfMTIwMDUw.jpeg
categories:
  - 工具
tags:
  - Python
  - SecureCRT
  - Linux
draft: false
---

在笔记本上的虚拟机中装了一个Linux系统，有一段时间没用，突然要用到时发现，密码已经不记得了。
还好之前有用过**secureCRT**软件连接过**Linux**，那么就能轻松地使用**python**找回密码啦。

## 最终效果
红框所指的位置就是密码了

![](https://ghjayce.github.io/asset/blog/RcU4wCd3Mk2Atx0pD4i3MjAxODAzMTdfMTEyMDAw.jpeg)

## 准备
下载安装[python](https://www.python.org/downloads/)，并配置系统全局变量。这里我用的是**python 2.7**

### python依赖包
下载python解密依赖包：https://pypi.python.org/pypi/pycrypto
解压文件，用命令行工具进入解压后的目录，执行下面命令
```shell
python setup.py build
python setup.py install
```
执行如果出现下图这种情况的

![](https://ghjayce.github.io/asset/blog/fD4JSCAXf8oKpphUaQadMjAxODAzMTdfMTEzNjAw.jpeg)

还有另一种方法可以安装
到这里下载对应自己环境的版本 http://www.voidspace.org.uk/python/modules.shtml#pycrypto
> 我下载的版本是**PyCrypto 2.6 for Python 2.7 32bit**

下载好了直接运行安装

## 开始找回
找到SecureCRT存储密码的位置
`用户名\AppData\Roaming\VanDyke\Config\Sessions\`

或者

`软件目录下\Data\Settings\Config\Sessions\`

> 目录中能看你的回话配置文件，我的是`10.0.0.100.ini`

复制下面代码，保存文件到上面的这个目录，起名`secureDecode.py`
```python
from Crypto.Cipher import Blowfish
import argparse
import re

def decrypt(password) :
    c1 = Blowfish.new('5F B0 45 A2 94 17 D9 16 C6 C6 A2 FF 06 41 82 B7'.replace(' ','').decode('hex'), Blowfish.MODE_CBC, '\x00'*8)
    c2 = Blowfish.new('24 A6 3D DE 5B D3 B3 82 9C 7E 06 F4 08 16 AA 07'.replace(' ','').decode('hex'), Blowfish.MODE_CBC, '\x00'*8)
    padded = c1.decrypt(c2.decrypt(password.decode('hex'))[4:-4])
    p = ''
    while padded[:2] != '\x00\x00' :
        p += padded[:2]
        padded = padded[2:]
    return p.decode('UTF-16')

REGEX_HOSTNAME = re.compile(ur'S:"Hostname"=([^\r\n]*)')
REGEX_PASWORD = re.compile(ur'S:"Password"=u([0-9a-f]+)')
REGEX_PORT = re.compile(ur'D:"\[SSH2\] Port"=([0-9a-f]{8})')
REGEX_USERNAME = re.compile(ur'S:"Username"=([^\r\n]*)')

def hostname(x) :
    m = REGEX_HOSTNAME.search(x)
    if m :
        return m.group(1)
    return '???'

def password(x) :
    m = REGEX_PASWORD.search(x)
    if m :
        return decrypt(m.group(1))
    return '???'

def port(x) :
    m = REGEX_PORT.search(x)
    if m :
        return '-p %d '%(int(m.group(1), 16))
    return ''

def username(x) :
    m = REGEX_USERNAME.search(x)
    if m :
        return m.group(1) + '@'
    return ''

parser = argparse.ArgumentParser(description='Tool to decrypt SSHv2 passwords in VanDyke Secure CRT session files')
parser.add_argument('files', type=argparse.FileType('r'), nargs='+',
    help='session file(s)')

args = parser.parse_args()

for f in args.files :
    c = f.read().replace('\x00', '')
    print f.name
    print "ssh %s%s%s # %s"%(port(c), username(c), hostname(c), password(c))
```

接着用命令行工具进入该目录，运行下面命令
```shell
python secureDecode.py 10.0.0.100.ini
```
最后成功找回密码

![](https://ghjayce.github.io/asset/blog/RcU4wCd3Mk2Atx0pD4i3MjAxODAzMTdfMTEyMDAw.jpeg)

## 参考
- [找回SecureCRT密码](http://blog.csdn.net/nickwong_/article/details/52373279)
- [pycrypto模块安装失败如何解决](http://blog.chinaunix.net/uid-24917554-id-3476396.html)