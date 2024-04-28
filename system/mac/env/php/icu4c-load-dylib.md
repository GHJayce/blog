---
title: Mac系统多个php版本环境下未找到icu4c动态库的解决过程
date: 2024-04-26T15:24:57+08:00
updateDate: 2024-04-28T19:59:00+08:00
slug: system/mac/env/php/icu4c-load-dylib
image: https://ghjayce.github.io/asset/blog/xLrs7QpeKQhM85mLcIcqMjAyNDA0MjZfMTUzMTU1.png
categories:
  - 操作系统
tags:
  - PHP
  - Mac
  - ICU4C
  - 动态链接库
draft: false
---
## 阅读说明

### 环境&软件版本
- MacOS Big Sur v11.0.1
- Homebrew v4.0.21
- GNU Make v3.81
- PHP v7.3.33、v7.4.33、v8.2
- ICU4C v72.1、v73.2、v74.2

### 背景
我的Mac环境早些时候用Homebrew安装了多个php的版本，php 7.3、7.4、8.2都有，为了区分多个php版本，我在`~/.bash_profile`中用alias做了处理，就像这样：

```bash
alias php73="/usr/local/opt/php@7.3/bin/php"
alias php74="/usr/local/opt/php@7.4/bin/php"
alias php82="/usr/local/opt/php@8.2/bin/php"
```

最近要给某个php项目`composer install`时需要用到php7.3，结果出现了报错：

```bash
dyld: Library not loaded: /usr/local/opt/icu4c/lib/libicui18n.72.dylib
  Referenced from: /usr/local/opt/php@7.3/bin/php
  Reason: image not found
zsh: abort      /usr/local/opt/php@7.3/bin/php --version
```

而8.2的版本执行正常，因为此时安装的icu4c的版本是73.2的。

7.3、7.4需要的icu4c版本是72的，所以它们在执行`php --verion`时都出现了报错。

> 有可能是因为安装php8，或者别的软件的时候，brew把icu4c的版本给升级上去造成的。
> 
> 升级就升级呗，不曾想旧的版本也没给我留着，好好好（brew好感度-1）。

### 疑问
- Q1：这是个关于什么问题的错误？
- Q2：这个错误是谁抛出的？操作系统还是应用程序（php）？
- Q3：这个路径是写死在应用程序（php）中的吗？有办法配置或修改吗？

## 解答疑问

先来说说这是个什么样的错误：

这是应用程序依赖某个动态链接库（Dynamic Link Libraries），而这个动态链接库的加载出现了问题，也就是说在路径下找不到这个文件了，其实不查资料光看报错的内容也能猜到一些。

那错误内容都有哪些含义或者信息呢？
- dyld: Library not loaded：表明动态链接器未能加载一个或多个指定的动态库。
- Referenced from：指出哪个文件（程序或动态库）引用了缺失的动态库。
- Reason：提供无法加载动态库的原因，常见的原因包括：
	- image not found：指定的动态库在文件系统中找不到。
	- image not loadable：找到了动态库，但由于某些原因（如格式错误、权限问题等）无法加载。
- did not find：列出了缺失的动态库的具体路径或名称。

这个错误实际上是Mac操作系统报告的错误，表明动态链接器（dynamic linker，即 dyld）在尝试加载程序所需的一个或多个动态库时遇到了问题。

你可能会有疑问了，动态库的后缀不都是`.so`后缀的吗？

查了一下资料是这么说的：

UNIX系统动态库文件的后缀通常是`.so`，尽管Mac系统是基于UNIX的，但`.so`文件在Mac系统上不如`.dylib`常见。如果你发现有`.so`文件，那可能是从UNIX或Linux移植过来的应用程序中携带的。

`.dylib`是Mac系统中最常用的动态链接库后缀，并且从macOS 10.4（Tiger）开始，`.dylib`取代了旧的`.so`后缀，成为系统的默认格式。

## 解决方案
解决也是很简单，只要让程序找到动态库文件不就行了。

> 我自己用的是方案二的方法二解决的，详细可看[解决过程](#解决过程)。

### 方案一 解铃还须系铃人homebrew
#### 方法一 官方的多版本 Formula
Homebrew允许安装某些软件的特定版本，如果官方提供了这些版本的Formula。可以使用以下命令：
```bash
brew search icu4c
```
如果找到了需要的版本，执行安装即可
```bash
brew install icu4c@72
```


这方法我试了，没有旧版本，只有icu4c的最新版
```bash
==> Formulae
icu4c ✔
```
手贱的我试了`brew reinstall icu4c`。

结果给我安装了最新版icu4c 74.2，这下好了php82也不能用了。。
#### 方法二 从Formula的Git历史版本安装
如果官方提供了多个版本的Formula，但需要的版本不在列表中，可以通过查看Formula的Git提交历史来找到对应版本的Formula，并检出（checkout）到那个特定的提交。

以下操作步骤：

```bash
# 1. 进入Homebrew的Formula目录：
cd $(brew --prefix)/Homebrew/Library/Taps/homebrew/homebrew-core/Formula

# 2. 使用`git log`查看`icu4c.rb`的提交记录：
git log --follow icu4c.rb

# 3. 检出到您需要的特定版本对应的提交：
git checkout -b icu4c-desired-version <commit-hash>

# 4. 重新安装 ICU4C
brew reinstall ./icu4c.rb

# 5. 如果需要，使用`brew switch`切换到该版本：
brew switch icu4c <desired-version>
```

这个方法我没有试完，因为操作的过程很卡，有想尝试的朋友可以试一下。

#### 方法三 自定义本地 Formula
如果需要的版本不在Homebrew的官方仓库中，可以创建一个自定义的Formula文件。

相关步骤：

1. 创建一个自定义的Formula文件，例如 `icu4c@72.rb`。
2. 编写Formula文件，指定软件的描述、URL、版本号、SHA256签名等信息。可以参考现有的Formula文件来编写。
3. 安装自定义的Formula：`brew install /path/to/icu4c@72.rb`
4. 如果需要，创建软链接以解决依赖问题。

同样，没试过。

### 方案二 自己下载icu4c编译安装
[unicode-org/icu下载地址](https://github.com/unicode-org/icu/releases?page=3)

编译安装过程：
```bash
tar -zxvf ./icu4c-72_1-src.tgz
cd icu/source
./configure --prefix=/usr/local/Cellar/icu4c/72.1
make && make install
```

将动态库文件拷贝到指定目录下：
```bash
cd /usr/local/Cellar/icu4c/72.1/lib
cp ./libicui18n.72.dylib /usr/local/opt/icu4c/lib/libicui18n.72.dylib
cp ./libicuuc.72.1.dylib /usr/local/opt/icu4c/lib/libicuuc.72.dylib
cp ./libicudata.72.1.dylib /usr/local/opt/icu4c/lib/libicudata.72.dylib
cp ./libicuio.72.1.dylib /usr/local/opt/icu4c/lib/libicuio.72.dylib
```

检查一下效果
```bash
php73 --version
# 执行结果：
dyld: Library not loaded: libicuuc.72.dylib

Referenced from: /usr/local/Cellar/icu4c/74.2/lib/libicui18n.72.dylib

Reason: image not found

zsh: abort /usr/local/opt/php@7.3/bin/php --version
```

#### 方法一 DYLD_LIBRARY_PATH
还是报错，还是没找到路径对吧，最简单的处理方法是，设置`DYLD_LIBRARY_PATH`环境变量，动态链接器搜索动态库文件时，如果在其他位置都没找到的情况下，会在你设定变量的目录下寻找动态库。

设置在`.bash_profile`、`.bashrc`或`.zshrc`都可以（**以下命令可直接照抄**）：
```bash
echo 'export DYLD_LIBRARY_PATH="/usr/local/opt/icu4c/lib/"' >> ~/.bash_profile
source ~/.bash_profile
```

来检查一下效果：
```bash
php73 --version
# 执行结果：
PHP 7.3.33 (cli) (built: Jun  8 2023 15:56:42) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.3.33, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.3.33, Copyright (c) 1999-2018, by Zend Technologies
```

> 动态链接器搜索的位置一般有：
> - 可执行文件所在的目录。
> - 系统默认的库目录，如 `/usr/lib` 和 `/usr/local/lib`。
> - 由环境变量 `DYLD_LIBRARY_PATH` 或 `LD_LIBRARY_PATH` 指定的目录（在 Linux 中是 `LD_LIBRARY_PATH`）。

#### 方法二 install_name_tool
相对麻烦的做法，直接修改动态库文件所在的目录。

根据错误内容，看下`libicui18n.72.dylib`、`libicuuc.72.dylib`、`libicuio.72.dylib`依赖的所有动态库。

```bash
otool -L /usr/local/Cellar/icu4c/74.2/lib/libicui18n.72.dylib
# 执行结果：
/usr/local/Cellar/icu4c/74.2/lib/libicui18n.72.dylib:
	libicui18n.72.dylib (compatibility version 72.0.0, current version 72.1.0)
	libicuuc.72.dylib (compatibility version 72.0.0, current version 72.1.0)
	libicudata.72.dylib (compatibility version 72.0.0, current version 72.1.0)
	/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1292.100.5)
	/usr/lib/libc++.1.dylib (compatibility version 1.0.0, current version 905.6.0)
```

将如上的依赖库`libicuuc.72.dylib`、`libicudata.72.dylib`、`libicui18n.72.dylib`加上一个`@loader_path`特殊前缀，使用`install_name_tool`命令进行修改（回答了Q3的问题）。

因为其他文件操作也一样，所以这里直接贴完整的操作过程（**照抄作业即可**）：
```bash
cd /usr/local/opt/icu4c/lib

# libicui18n.72.dylib
install_name_tool -change libicuuc.72.dylib @loader_path/libicuuc.72.dylib ./libicui18n.72.dylib
install_name_tool -change libicudata.72.dylib @loader_path/libicudata.72.dylib ./libicui18n.72.dylib

# libicuuc.72.dylib
install_name_tool -change libicudata.72.dylib @loader_path/libicudata.72.dylib ./libicuuc.72.dylib

# libicuio.72.dylib
install_name_tool -change libicudata.72.dylib @loader_path/libicudata.72.dylib ./libicuio.72.dylib
install_name_tool -change libicuuc.72.dylib @loader_path/libicuuc.72.dylib ./libicuio.72.dylib
install_name_tool -change libicui18n.72.dylib @loader_path/libicui18n.72.dylib ./libicuio.72.dylib
```

再来检查一下效果：
```bash
php73 --version
# 执行结果：
PHP 7.3.33 (cli) (built: Jun  8 2023 15:56:42) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.3.33, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.3.33, Copyright (c) 1999-2018, by Zend Technologies
```

##### 命令说明

`otool`是macOS系统上的一个命令行工具，它用于显示和操作对象文件（包括静态库和动态库）。并提供了多种功能，可以帮助你检查和修改 Mach-O 对象文件的头部信息、数据段、符号表等。
- `-L`选项用于显示Mach-O文件（如动态库`.dylib`或可执行文件）所依赖的所有动态库。

`install_name_tool`是macOS上的一个命令行工具，用于修改Mach-O可执行文件和动态链接库（dynamic libraries，即`.dylib`文件）中的安装名称（install names）。这些安装名称是动态库或框架的标识符，用于运行时查找和加载依赖项。
- `-change` 选项用于更改动态库的安装名称。
	- 这在以下情况下非常有用：
		- 当你想要更新一个动态库的路径，但是可执行文件或另一个动态库中的安装名称仍然指向旧路径时。
		- 当你将一个应用程序或库移动到一个新位置，需要更新所有引用该库的路径时。
	- 基本语法：`install_name_tool -change old_name new_name file`
		- `old_name`：当前的安装名称，即你想要更改的旧路径或名称。
		- `new_name`：新的安装名称，即你想要设置的新路径或名称。
		- `file`：包含要更改的安装名称的 Mach-O 文件（可执行文件或动态库）。

`@loader_path` 是一个特殊的前缀，用于指定相对于动态链接器加载程序的路径。当你在动态库中引用其他动态库时，可以使用 `@loader_path` 来告诉动态链接器在加载当前动态库的相同路径下查找依赖的动态库。

## 解决过程

说的容易做起来难，以下是自己在一知半解的情况下的折腾过程，最后也是解决了问题：

> 本来是当时解决完事后打的草稿，后面打算改一下，看了下还挺有趣，不改了，浅浅记录一下。

因为会用到`install_name_tool`对动态链接库进行修改，使用ln的方式会改到原文件，所以做个拷贝到目标目录，直接在拷贝文件上进行修改。
```bash
cd /usr/local/Cellar/icu4c/72.1/lib
cp ./libicui18n.72.1.dylib /usr/local/opt/icu4c/lib/libicui18n.72.dylib
cp ./libicuuc.72.1.dylib /usr/local/opt/icu4c/lib/libicuuc.72.dylib
cp ./libicudata.72.1.dylib /usr/local/opt/icu4c/lib/libicudata.72.dylib
cp ./libicuio.72.1.dylib /usr/local/opt/icu4c/lib/libicuio.72.dylib
```
执行观察下
```bash
php73 --version
# 执行结果：
dyld: Library not loaded: libicuuc.72.dylib
  Referenced from: /usr/local/Cellar/icu4c/74.2/lib/libicui18n.72.dylib
  Reason: image not found
zsh: abort      /usr/local/opt/php@7.3/bin/php --version
```

发现还是报错，balabala 先爬帖，看有没有别的解决方法。

发现`DYLD_LIBRARY_PATH`可以解决问题，但我不想用这种方式。

然后继续折腾，折腾的麻了，想放弃直接`brew reinstall php@7.3`重装得了，结果跑了几次都raw.githubusercontent.com连接超时，重试了几次，连上了，直到遇到：
```bash
brew reinstall php@7.3
# 执行结果：
...
==> Installing dependencies for httpd: openssl@3, brotli, libnghttp2 and pcre2
==> Installing httpd dependency: openssl@3
Warning: A newer Command Line Tools release is available.
Update them from Software Update in System Preferences.

If that doesn't show you any updates, run:
  sudo rm -rf /Library/Developer/CommandLineTools
  sudo xcode-select --install

Alternatively, manually download them from:
  https://developer.apple.com/download/all/.
You should download the Command Line Tools for Xcode 13.2.1.

==> perl ./Configure --prefix=/usr/local/Cellar/openssl@3/3.1.1 --openssldir=/usr/local/etc/openssl@3 --libdir=/usr/local/Cellar/openss
==> make
==> make install MANDIR=/usr/local/Cellar/openssl@3/3.1.1/share/man MANSUFFIX=ssl
==> make test
Error: Empty installation
```
再重试几次，好家伙，还是一样，直接劝退了。

回头继续折腾修改动态库路径的这个方向。

冷静思考下，网上的文章都说cp完那些动态库就可以了，怎么到了我的环境就不行？

好像也不是不行，是行的，我逐个cp动态库以后再执行`php73 --version`能够看到错误内容有发生变化，那不就说明这个操作是有效的吗？那只要仔细看最后一个错误内容，说不定就能发现点什么

对比了一下两次的结果：
```bash
php73 --version
# 执行结果：
dyld: Library not loaded: /usr/local/opt/icu4c/lib/libicuio.72.dylib

Referenced from: /usr/local/opt/php@7.3/bin/php

Reason: image not found

zsh: abort /usr/local/opt/php@7.3/bin/php --version
```
四个文件都cp完再以后再执行的错误内容：
```bash
php73 --version
# 执行结果：
dyld: Library not loaded: libicuuc.72.dylib

Referenced from: /usr/local/Cellar/icu4c/74.2/lib/libicui18n.72.dylib

Reason: image not found

zsh: abort /usr/local/opt/php@7.3/bin/php --version
```
乍眼一看又是`libicui18n`，好像又回到了第一个错误一样，仔细观察可以发现两次的Referenced from不同，Library not loaded的内容也不同。

但是有什么关联呢？一时想不明白，先去卫生间小解一下（日常想到头破憋尿鳖的不行才去）。

回来的路上灵光一现，既然可以`otool -L /usr/local/opt/php@7.3/bin/php`，同理，同样也可以`otool -L /usr/local/Cellar/icu4c/74.2/lib/libicui18n.72.dylib`

赶紧快步走回去试一下：
```bash
cd /usr/local/opt/icu4c/lib
otool -L ./libicui18n.72.dylib
# 执行结果：
./libicui18n.72.dylib:
	libicui18n.72.dylib (compatibility version 72.0.0, current version 72.1.0)
	libicuuc.72.dylib (compatibility version 72.0.0, current version 72.1.0)
	libicudata.72.dylib (compatibility version 72.0.0, current version 72.1.0)
	/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1292.100.5)
	/usr/lib/libc++.1.dylib (compatibility version 1.0.0, current version 905.6.0)
```

对比icui18n.74.dylib，观察有什么不同
```bash
otool -L /usr/local/Cellar/icu4c/74.2/lib/libicui18n.74.2.dylib
# 执行结果：
/usr/local/Cellar/icu4c/74.2/lib/libicui18n.74.2.dylib:
	/usr/local/opt/icu4c/lib/libicui18n.74.dylib (compatibility version 74.0.0, current version 74.2.0)
	@loader_path/libicuuc.74.dylib (compatibility version 74.0.0, current version 74.2.0)
	@loader_path/libicudata.74.dylib (compatibility version 74.0.0, current version 74.2.0)
	/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1292.100.5)
	/usr/lib/libc++.1.dylib (compatibility version 1.0.0, current version 905.6.0)
```

可以明显看到，74中的相关依赖是多了`@loader_path`，而72中没有。

大胆猜测一下，说不定正是因为i18n依赖了别的库，而库的地址，就是因为少了@loader_path所以才会有最后一个错误的提示，这样是不是就能说得通了？

马上着手改一下试试
```bash
cd /usr/local/opt/icu4c/lib
install_name_tool -change libicuuc.72.dylib @loader_path/libicuuc.72.dylib ./libicui18n.72.dylib
otool -L ./libicui18n.72.dylib
# 执行结果：
./libicui18n.72.dylib:
	libicui18n.72.dylib (compatibility version 72.0.0, current version 72.1.0)
	@loader_path/libicuuc.72.dylib (compatibility version 72.0.0, current version 72.1.0)
	libicudata.72.dylib (compatibility version 72.0.0, current version 72.1.0)
	/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1292.100.5)
	/usr/lib/libc++.1.dylib (compatibility version 1.0.0, current version 905.6.0)

```
再来执行php看下
```bash
php73 --version
# 执行结果：
dyld: Library not loaded: libicudata.72.dylib
  Referenced from: /usr/local/Cellar/icu4c/74.2/lib/libicui18n.72.dylib
  Reason: image not found
zsh: abort      /usr/local/opt/php@7.3/bin/php --version
```
很好，错误内容有新的变化了，不再是`libicuuc.72.dylib`了，而`libicudata.72.dylib`正是下一个要依赖的动态库，说明修改起作用了，继续改
```bash
install_name_tool -change libicudata.72.dylib @loader_path/libicudata.72.dylib ./libicui18n.72.dylib
```
再执行php观察返回结果
```bash
php73 --version
# 执行结果：
dyld: Library not loaded: libicudata.72.dylib
  Referenced from: /usr/local/Cellar/icu4c/74.2/lib/libicuuc.72.dylib
  Reason: image not found
zsh: abort      /usr/local/opt/php@7.3/bin/php --version
```
很好，这次Referenced from是`libicuuc.72.dylib`了，说明改对了，接下来就是重复这个过程，直到把icu4c所有动态库的依赖地址都改掉，再来看下结果：
```bash
php73 --version
# 执行结果：
PHP 7.3.33 (cli) (built: Jun  8 2023 15:56:42) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.3.33, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.3.33, Copyright (c) 1999-2018, by Zend Technologies
```
至此，问题终于解决了，总算没白费磕了那么多时间，又可以写一篇文章了😎。


## 参考
- [macOS 开发中动态库问题剖析-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/2052357)
- [php 执行报错 icu4c错误 - 蓝静空 - 博客园](https://www.cnblogs.com/iifeng/p/17714722.html)
- [解决mac下:dyld: Library not loaded: /usr/local/opt/icu4c/lib/libicui18n.64.dylib - 笨笨韩 - 博客园](https://www.cnblogs.com/benbenhan/articles/16888353.html)
- [Importing Enterprise Application Projects into Oracle Developer Studio IDE](https://www.oracle.com/technetwork/articles/servers-storage-dev/import-enterprise-studio-ide-1689617.html)
- [MacOS 遇到 dyld: Library not loaded: xxx.dylib 的解决方案 - 简书](https://www.jianshu.com/p/6625fd5c72b9)
- [MAC 升级 Homebrew 导致icu4c库依赖版本问题\_failed to download resource "icu4c.rb-CSDN博客](https://blog.csdn.net/chechengxue/article/details/135708333)
- [解决mac下:dyld: Library not loaded: /usr/local/opt/icu4c/lib/libicui18n.64.dylib\_dyld[72382]: library not loaded: libicui18n.64.dyl-CSDN博客](https://blog.csdn.net/jmdxin/article/details/114970739)
- [Dynamic Library Usage Guidelines](https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/DynamicLibraryUsageGuidelines.html#//apple_ref/doc/uid/TP40001928-SW10)