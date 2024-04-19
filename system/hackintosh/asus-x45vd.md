---
title: "华硕x45vd安装黑苹果Yosemite 10.10.3记录"
date: 2017-11-04T21:03:00+08:00
updateDate: 2017-11-04T21:03:00+08:00
slug: system/hackintosh/asus/x45vd
image: https://ghjayce.github.io/asset/blog/dPMjucATggvoLXmJZXz5MjAxNzExMDRfMjEwMzAw.jpeg
categories:
  - 操作系统
tags:
  - 黑苹果
  - 华硕
  - Windows
  - MacOS
draft: false
---

## 准备工作

Yosemite10.10.3懒人版
链接: https://pan.baidu.com/s/1i5aOBOT 密码: hukg


安装环境：
- windows10 64位
- 变色龙 2.3

配置图如下：

![ASUS X45VD](https://ghjayce.github.io/asset/blog/aAFyGS4dAC5TUaJCm8jfMjAxNzExMDRfMTk0NTMz.png)


## 开启ahci模式

否则无法进入安装界面。或者打免ahci的补丁。


由于我装好的win系统用的是ide，开启ahci模式，我的win系统就不能正常进入。需要设置注册表，方法如下

### Win7下开启ACHI

- 单击“开始”按钮，在搜索框中键入“regedit”，按下回车键，打开“注册表编辑器”窗口。Windows7虽然在“开始”菜单默认不显示“运行”命令，但实际上可用搜索框代替这一功能（或者直接按下Windows键+R键再输入）。 

- 在“注册表编辑器”窗口左侧标题栏定位至HKEY_LOCAL_MACHINE\SYSTEM\ CurrentControlSet\services\msahci分支，然后在右侧窗口，双击“Start”。 

- 在打开的“编辑DWORD值”对话框，将“数值数据”框中的值由3改为数字0，单击“确定”按钮。 


### win 10 开启achi方式

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\storahci`下面的`StartOverride`里面的0键值改为0，默认是3


进入安装界面，`-v -f`跑啰嗦模式，很顺利地进入安装界面


安装完成后进入Mac系统盘，继续跑啰嗦模式。


## 问题集合

遇到第一个问题

![这里写图片描述](https://ghjayce.github.io/asset/blog/ZMMaj0LwWAgQVPQn3DxxMjAxNzExMDRfMTk0OTM1.jpeg)

解决：删除 S/L/E 文件夹下面的
- AppleIntelCPUPowerManagement.kext
- AppleIntelCPUPowerManagementClient.kext

第二个问题：

![这里写图片描述](https://ghjayce.github.io/asset/blog/iIEzqO0dluVewMwZPOe3MjAxNzExMDRfMTk1NzMx.jpeg)

解决方法：	
- 威锋五国帖 [Q：23](http://bbs.feng.com/read-htm-tid-4330923.html)
- 远景五国贴 [Q：10](http://bbs.pcbeta.com/forum.php?mod=viewthread&tid=1280262)

尝试方案：
- `-v -f -x`进入啰嗦模式。`-x`意思是不加载任何驱动
	- 不再出现第二个问题，但是跑啰嗦代码非常非常非常的慢。并且出现新的问题
	- 失败


---


中间各种爬帖，各种碰壁，各种尝试....

最终还是让我进了苹果系统！哈哈，这种成就感真的是难以描述，很奇妙。

![这里写图片描述](https://ghjayce.github.io/asset/blog/HnG8gBcoFHSpzLnSv7ybMjAxNzExMDRfMjMzNzIz.jpeg)

![这里写图片描述](https://ghjayce.github.io/asset/blog/NwD35RqDlyvuPcf7cTD5MjAxNzExMDRfMjMzODAz.jpeg)

---

下面是我爬帖按照帖子中的内容去做的尝试，其中这些文件的牵连，应该是影响到进入系统的关键，也许起作用的并不是全部，但也值得一试。


- SNB HD 3000集显，/Extra/目录下加入MacBookPro8.1 的smbios.plist

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>SMfamily</key>
	<string>MacBook Pro</string>
	<key>SMproductname</key>
	<string>MacBookPro8,1</string>
	<key>SMboardproduct</key>
	<string>Mac-94245B3640C91C81</string>
	<key>SMserial</key>
	<string>C02F93FQDH2G</string>
	<key>SMbiosversion</key>
	<string>MultiBeast.tonymacx86.com</string>
</dict>
</plist>
```

- 删除文件
	- 删了全部`/System/Library/Extensions/AppleIntel****.****` 显卡驱动(记得备份) 
	- 删了全部`/System/Library/Extensions/ATI****.****` 显卡驱动(记得备份)
	- 删了全部`/System/Library/Extensions/Geforce****.****` 显卡驱动(记得备份) 
	- 删了全部`/System/Library/Extensions/NVDA****.****` 显卡驱动(记得备份)


补充文件
- org.chameleon.Boot.plist

```xml
<key>Kernel Flags</key>
<string>-v -f cpus=1</string>
<key>GraphicsEnabler</key>
<string>No</string>
```

- S/L/E 目录下加入
	- FakeSMC.kext
	- NullCPUPowerManagement.kext
	- **7系主板已经修改好的HD3000驱动**


所有文件
链接: https://pan.baidu.com/s/1kVj7Wiz 密码: nxk9


## 爬帖链接集合

- [也是ntfs volume name问题，有图](http://bbs.pcbeta.com/forum.php?mod=viewthread&tid=1081866&highlight=)
- [被整了一天了~NTFS volume name ***，version 3.1 问题](http://bbs.feng.com/read-htm-tid-3911873-page-1.html)
- [☪☪☪新手必看，初级知识普及贴，及常见五国错误解决办法 ](http://bbs.feng.com/read-htm-tid-4330923.html)
- [黑苹果常见五国问题解决](http://bbs.pcbeta.com/forum.php?mod=viewthread&tid=1280262)
- [Missing Bluetooth Controller Transport!](http://bbs.pcbeta.com/viewthread-1451360-1-1.html)
- [求助，安装完系统后卡在missing bluetooth controller transport](http://www.memacx.com/thread-5406-1-1.html)
- [安装时卡在Resetting IOCatalogue处](http://bbs.pcbeta.com/viewthread-1256245-1-1.html?spm=0.0.0.0.KTJpEh)