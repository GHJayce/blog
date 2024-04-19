---
title: "windows自定义程序到右键菜单"
date: 2016-12-25T10:34:34+08:00
updateDate: 2016-12-25T10:34:34+08:00
slug: system/windows/regedit/right-click-menu/custom
image: https://ghjayce.github.io/asset/blog/jhPbumQV8Q1Tn3JTsxQaMjAxNjEyMjVfMTAzNDM0.jpeg
categories:
  - 操作系统
tags:
  - 右键菜单
  - 注册表
  - Windows
draft: false
---
打开注册表`regedit`

要在背景上设置右键菜单的
依次进入
- `HKEY_CLASSES_ROOT`
- `Directory`
- `Background`
- `shell`

要在选中文件时设置右键菜单的
依次进入
- `HKEY_CLASSES_ROOT`
- `Directory`
- `shell`


## 注册文件示例

> 下面内容按照实际进行替换。

### 背景的右键菜单示例
保存下面代码为`example.reg`文件，打开运行即可
```ini
Windows Registry Editor Version 5.00

; 这里是右键菜单注册项
[HKEY_CLASSES_ROOT\Directory\Background\shell\cmder]
; @ 是右键菜单的名称
@="open Cmder"
; Extended 是要按shift+鼠标右键才会显示，不需要的话去除下面一行代码即可
"Extended"=""
; Icon 表示右键菜单时显示的图标
"Icon"="C:\\cmder\\icons\\cmder.ico"

; 这里是右键菜单注册项的重要子项
[HKEY_CLASSES_ROOT\Directory\Background\shell\cmder\command]
; 下面表示右键菜单软件的目录
@="C:\\cmder\\Cmder.exe"
```

### 选中文件右键菜单示例
使用方法：保存下面代码为`name.reg`文件，打开运行即可
```ini
Windows Registry Editor Version 5.00
[HKEY_CLASSES_ROOT\*\shell\SublimeText3]
; 右键菜单名称
@="用 SublimeText3 打开"
; 显示的图标   后面 的， 逗号应该是用使用软件的图标
"Icon"="D:\\Program Files\\Sublime Text 3\\sublime_text.exe,0"

[HKEY_CLASSES_ROOT\*\shell\SublimeText3\command]
; 软件的路径
@="D:\\Program Files\\Sublime Text 3\\sublime_text.exe %1"


[HKEY_CLASSES_ROOT\Directory\shell\SublimeText3]
; 右键菜单名称
@="用 SublimeText3 打开"
; 图标的路径
"Icon"="D:\\Program Files\\Sublime Text 3\\sublime_text.exe,0"

; 软件的路径
[HKEY_CLASSES_ROOT\Directory\shell\SublimeText3\command]
@="D:\\Program Files\\Sublime Text 3\\sublime_text.exe %1"
```

#### 另外一种方法
使用方法：放到和软件同目录，保存下面代码为`name.inf`，右键安装即可。【推荐】

> 和上面方法的区别就是，文件放置的目录不一样，这种方法要放到和软件同目录下，上面的方法则是任意目录。

```ini
[Version]
Signature="$Windows NT$"

[DefaultInstall]
; 注册表名称
AddReg=SublimeText3

[SublimeText3]
; 菜单显示的名称
hkcr,"*\\shell\\SublimeText3",,,"用 SublimeText3 打开"
; 软件路径
hkcr,"*\\shell\\SublimeText3\\command",,,"""%1%\sublime_text.exe"" ""%%1"" %%*"
; 显示的名称
hkcr,"Directory\shell\SublimeText3",,,"用 SublimeText3 打开"
; 软件的图标
hkcr,"*\\shell\\SublimeText3","Icon",0x20000,"%1%\sublime_text.exe, 0"
; 软件的路径
hkcr,"Directory\shell\SublimeText3\command",,,"""%1%\sublime_text.exe"" ""%%1"""
```

## 一个小工具
生成windows右键菜单的注册表文件，通过填写几个选项就能得到一个注册表文件。（太方便了:satisfied:）

- [GHJayce/RightClickMenu](https://github.com/GHJayce/RightClickMenu)
- [在线体验](https://ghjayce.github.io/RightClickMenu/#/)