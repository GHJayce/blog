---
title: "将Sublime Text 添加到右键菜单的方法"
date: 2016-12-26T10:34:34+08:00
updateDate: 2016-12-26T10:34:34+08:00
slug: system/windows/regedit/right-click-menu/sublime-text
image: https://ghjayce.github.io/asset/blog/WKRr6SlzDfcw9eTWRv1EMjAxNjEyMjZfMTAzNDM0.jpeg
categories:
  - 操作系统
tags:
  - Sublime Text
  - 编辑器
  - 右键菜单
  - 注册表
  - Windows
draft: false
---

## 方法一

把以下代码，复制到**SublimeText3的文件目录**，然后重命名为：**安装右键菜单.inf**，然后右击安装就可以了。

> PS：重命名文件之前，需要先在工具--文件夹选项，查看中，把隐藏已知文件类型的扩展名前边的复选框不勾选。

```ini
[Version]
Signature="$Windows NT$"

[DefaultInstall]
AddReg=SublimeText3

[SublimeText3]
hkcr,"*\\shell\\SublimeText3",,,"用 SublimeText3 打开"
hkcr,"*\\shell\\SublimeText3\\command",,,"""%1%\sublime_text.exe"" ""%%1"" %%*"
hkcr,"Directory\shell\SublimeText3",,,"用 SublimeText3 打开"
hkcr,"*\\shell\\SublimeText3","Icon",0x20000,"%1%\sublime_text.exe, 0"
hkcr,"Directory\shell\SublimeText3\command",,,"""%1%\sublime_text.exe"" ""%%1"""
```

## 方法二

把以下代码，复制到SublimeText3的安装目录，然后重命名为：`sublime right menu.reg`，然后双击就可以了。

注意：
- 需要把下边的Sublime的安装目录，**替换成你的实际的Sublime安装目录。**
- 文件编码必须是**ANSI**，代码中的中文才会正常显示。

```ini
Windows Registry Editor Version 5.00
[HKEY_CLASSES_ROOT\*\shell\SublimeText3]
@="用 SublimeText3 打开"
"Icon"="D:\\Program Files\\Sublime Text 3\\sublime_text.exe,0"

[HKEY_CLASSES_ROOT\*\shell\SublimeText3\command]
@="D:\\Program Files\\Sublime Text 3\\sublime_text.exe %1"


[HKEY_CLASSES_ROOT\Directory\shell\SublimeText3]
@="用 SublimeText3 打开"
"Icon"="D:\\Program Files\\Sublime Text 3\\sublime_text.exe,0"

[HKEY_CLASSES_ROOT\Directory\shell\SublimeText3\command]
@="D:\\Program Files\\Sublime Text 3\\sublime_text.exe %1"
```

## 最后，附一个删除右键菜单的脚本吧。

把以下代码，复制到**SublimeText3的文件目录**，然后重命名为：`remove right menu.reg`，然后双击就可以了。

```ini
Windows Registry Editor Version 5.00
[-HKEY_CLASSES_ROOT\*\shell\SublimeText3]
[-HKEY_CLASSES_ROOT\Directory\shell\SublimeText3]
```

## 一个小工具
生成windows右键菜单的注册表文件，通过填写几个选项就能得到一个注册表文件。（太方便了:satisfied:）

- [GHJayce/RightClickMenu](https://github.com/GHJayce/RightClickMenu)
- [在线体验](https://ghjayce.github.io/RightClickMenu/#/)