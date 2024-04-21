---
layout:     post
title:      Switch 硬破和使用
subtitle:   Switch Cracked 
date:       2024-04-10
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - 生活
---

主要是给孩子做一个记录。

# 总结

这几天深入学习switch，了解一下所谓的switch的生态。

* [Switch Firmwares](https://darthsternie.net/switch-firmwares/)
* [大气层](https://github.com/Atmosphere-NX/Atmosphere/releases)
* [大气层整合包](https://codeberg.org/carcaschoi/Shallowsea/releases)

整合包是解决大气层还欠缺的各种工具，汉化等。让你开箱即用。


# 破解

switch的破解分成两种：硬破和软破。他们都是利用了芯片的漏洞（进入Nvidia T210的工程模式，RCM），运行自制代码。这就意味着可以引导到自制系统中，从而跳过NS自带系统的限制。

两者的区别在于

* 硬破：需要拆机焊接，需要有焊接经验，操作麻烦，好处是一次操作，终身受益。同时没有版本的限制，只要switch都支持硬破。
* 软破：就不需要焊接，通过一个特殊的装置（RCMLoader），可以进入到RCM中。

但在新版本的switch中，官方屏蔽了这种做法，所以只能在老机器中可行，目前说是2018年6月前的机器。且每次关机，都需要重新注入，引导到自制系统中，稍微麻烦。

可以简单理解，无论软解和硬解，最终的目的就是进入这个界面，后续的操作都一样，软解和硬解，都是支持虚拟系统，双系统。

![Hekate](/img/2024/son/Hekate.webp "Hekate")

软破买一个RCMLoader，成本在50RMB，硬破使用树莓派的方案，150块RMB。


## 硬破流程

第一步肯定是手工活，打开swith，焊接破解芯片。这个活，其实就只能专业厂商淘宝店来做。

第二步，才是搞软件，这才是我能做的事情。

理解系统的版本，开机启动的时候，有一个正版系统，一个虚拟系统

假设

* 正版系统：switch 固件版本 17.0.1，升级固件版本可以在线升级。
* 虚拟系统：switch 固件版本 17.0.1+大气层1.6.2，虚拟系统固件升级，只能是离线升级。离线升级就是在sd卡里放上更新的switch 固件版本 ，在虚拟系统里安装，实现更新。

新的游戏，会对switch 固件版本有依赖，所以有时候如果你想玩新的游戏，你就只能升级固件。在2024年，17.0.1+大气层1.6.2，应该是不会有固件升级的需求。2025年，据说switch 2.0 要发布。

大气层的版本支持的switch的版本是有对应关系的。如果你进入正版系统，升级版本到18.0.0，那么你的witch，就会变成砖头。

你进入到启动界面，无论虚拟系统，正版系统，都会无法启动。这个其实是和大气层的版本相关的。大气层的版本必须和主机的固件版本对应，才能让虚拟系统和正版系统启动。

挽救砖头也简单，你需要

* 升级sd卡的大气层系统从1.6.2到1.7.0
* 离线升级虚拟系统的switch 固件版本到18.0.0。（不升级虚拟系统的switch固件，也是可以正常使用）

具体的操作过程，我大概记录一下

* 从switch拔下sd卡
* 挂载电脑上，只保留**emuMMC** 和 **Nintendo** 两个文件夹，其余删除
* 下载1.7.0的大气层整合包，上面的地址可以直接下载，解压放到根目录。
* 下载switch 18.0.0 固件，解压后，在sd卡里建立自己命名文件夹，例如fireware，把固件放进去。
* 插入sd卡到switch，启动，这个时候，你就可以进入虚拟系统，这个时候，表示已经升级完成大气层系统到1.7.0。
* 进入虚拟系统后，通过daybreak，选择fireware的文件夹，进行虚拟系统的switch版本升级，把虚拟系统switch固件离线升级到18.0.0 。
* 最终的效果就是虚拟系统的switch的固件版本，和正版switch的固件版本一致。


# Switch 版本

游戏机和其他电子产品不一样，版本更新比较慢。

任天堂Switch 自 2017 年 3 月发布以来，至今已发布过四种机型：

* **普通版**：于 2017 年 3 月 3 日发布，是 Switch 的初代版本。续航能力约为 2.5-6 小时。
* **续航增强版**：于 2019 年 8 月 20 日发布，在普通版的基础上提升了续航能力，可达 4.5-9 小时。
* **Switch Lite**：于 2019 年 9 月 20 日发布，是 Switch 的掌机版本，不可连接电视。
* **OLED 款式**：于 2021 年 10 月 8 日发布，在续航增强版的基础上升级了屏幕、存储空间、支架等。

以下是四种机型的详细对比：

| 机型 | 发布日期 | 屏幕 | 处理器 | 内存 | 存储空间 | 续航能力 | 重量 | 价格 |
|---|---|---|---|---|---|---|---|---|
| 普通版 | 2017 年 3 月 3 日 | 6.2 英寸 LCD | NVIDIA Tegra X1 | 4GB LPDDR4 | 32GB | 2.5-6 小时 | 约 398g（含 Joy-Con） | ¥2099 |
| 续航增强版 | 2019 年 8 月 20 日 | 6.2 英寸 LCD | NVIDIA Tegra X1 | 4GB LPDDR4 | 64GB | 4.5-9 小时 | 约 398g（含 Joy-Con） | ¥2499 |
| Switch Lite | 2019 年 9 月 20 日 | 5.5 英寸 LCD | NVIDIA Tegra X1 | 4GB LPDDR4 | 32GB | 3-7 小时 | 约 275g | ¥1999 |
| OLED 款式 | 2021 年 10 月 8 日 | 7 英寸 OLED | NVIDIA Tegra X1 | 4GB LPDDR4 | 64GB | 4.5-9 小时 | 约 420g（含 Joy-Con） | ¥2499 |


我是2020年，给孩子购买的**续航增强版**。

# 破解

逛淘宝，会发现所谓软破和硬破，对于2024年，硬破是唯一选择，并且现在的硬破，以前硬破有所谓的芯片选择，所谓的国产芯片，由于主控，成本高，价格要400多，现在改成树莓派的主控，成本就降低到40，采用树莓派主控方案，拆开主机焊接破解芯片的方式破解，费用已经降低到150元以下，可以理解就是100块钱的手工费。

推荐师傅：郭师傅数码电玩DIY，淘宝抖音同名。



# 硬破软件

硬破盒子后，需要你准备一张SD卡，破解的系统都会装在这张卡上。

## Hekate 引导

Hekate是一款用于任天堂Switch游戏机的开源引导加载程序（Bootloader）。它主要用于解锁Switch的安全启动限制，允许用户执行各种高级操作。

Switch启动的时候，先到这个画面，让你选择

![Hekate](/img/2024/son/Hekate.webp "Hekate")

## 大气层 Atmosphere

大气层是Switch 中的免费开源的破解系统，目前也是唯一选择。树莓派芯片也只能用大气层系统，所以现在的破解机都只能在大气层上运行。

![Hekate-start](/img/2024/son/start.jpg "Hekate-start")

唯一的选择就是**大气层虚拟系统**。 

![choose](/img/2024/son/choose.jpg "choose")

大气层整合包，其实就是包括Hekate和大气层 Atmosphere，还有一堆的工具。

大气层的版本，可以通过[大气层版本号](https://github.com/Atmosphere-NX/Atmosphere/releases)

每个版本支持的switch的版本是不一样的。

* 大气层1.7.0 支持switch固件版本 18.0.0
* 大气层1.6.2 支持switch固件版本 17.0.1 （孩子当前使用的版本）

大气层本身是一个开源工具，本身还是不支持破解，你需要使用大气层整合包，才能实现真正的破解。

## Ban 机

也就是被任天堂拉黑的机器，被 Ban 后的机器无法与任天堂官方服务器联网，除此外和未 Ban 的机器无任何区别，判断是否 Ban 机也很简单，在正版系统下进入 eshop 看看是否能加载出来就知道了。

如果你不想被 Ban，后续玩破解游戏都需要在 虚拟系统 进行，在虚拟模式下进行的所有操作都是跟 NS 主机的原版系统 完全独立 的，所以只要你不作死和手贱，一般就不会被 Ban。

# 特斯拉和金手指

打游戏没法通关，采用作弊的方式。小孩不喜欢这样玩，所以就不需要研究这块。

# 上传游戏

需要一台windows电脑和一根Type-c的数据线。

要记住，内存卡任何文件不能出现中文

1：下载好游戏的安装包，打开switch相册

![相册](/img/2024/son/pic.jpg "相册")

2：点击DBI插件

![DBI](/img/2024/son/dbi.jpg "DBI")

3：点击右手柄x按键

![x](/img/2024/son/x.jpg "X")

屏幕显示，记得选择 **RUN MTP responder**

![mtp](/img/2024/son/mtp.jpg "mtp")

4:switch 用数据线连接电脑，电脑会出现弹窗，或者出现switch图标。

![install](/img/2024/son/install.jpg "install")

5：将游戏本体，更新包，复制到五号盘（Micro SD install）

一定要记住**只copy文件，不能是文件夹**

这是游戏拷贝和安装过程，不要太着急，你可以多个游戏一起copy过去，也可以一个游戏copy过去，安装成功，再装第二个。

等待进度条读取完成，按下右手柄**房子键**

![home](/img/2024/son/home.png "home")

# 删除软件

找到要删除的游戏，点击手柄上的 + 号，最后点击删除软件即可

选择：数据管理，选择删除软件就可以。

![delete](/img/2024/son/delete.jpg "delete")

# 系统版本和大气层版本

![version](/img/2024/son/version.jpg "version")

上图，应该是狠古老。目前主流的版本应该是

* 大气层1.7.0 支持switch 18.0.0
* 大气层1.6.2 支持switch 17.0.1 （孩子当前使用的版本）



# 游戏不能玩

![work](/img/2024/son/work.jpg "work")

# 更换内存卡

现在是一张512G的内存卡，家里还有一张1T的内存卡，如何把1T的卡也能用起来呢。

已经破解的系统，默认是从SD卡启动。

好像最简单的方式，就是把512G卡的内容，直接复制到1T的，应该也是没问题。如果两个SD卡都是采用exfat格式。

下面的记录过程，就当一个原理理解。

### 格式化

电脑格式化SD卡为exfat格式，分配单元大小为32k。选择32k，可以装更多游戏，如果是128k，那么性能更好，建议使用128k，我咨询了Gemini AI。

有用户建议用fat32格式，32k。文件系统不容易损坏。

![exfat](/img/2024/son/exfat.jpg "exfat")

### switch 支持exfat驱动

让swich识别exfat格式的卡。

由于我的switch已经破解使用中，所以肯定是带驱动，可以正常识别。

这个步骤可以跳过

### 大气层整合包

把大气层整合包解压，放到SD卡根目录下。

### 开机启动

把SD卡插入switch，进入Hekate，选择虚拟系统

![vir](/img/2024/son/vir.jpg "vir")

### 创建emuMMC

![emc](/img/2024/son/emc.jpg "emc")

这其实是从系统的正版固件，copy到sd卡里虚拟系统。

### SD卡文件

![sd](/img/2024/son/sd.jpg "sd")

### 成功

![ok](/img/2024/son/ok.jpg "ok")

### 检查

![open](/img/2024/son/open.jpg "open")

### 启动

![test](/img/2024/son/test.jpg "test")

### 选择大气层虚拟系统

![end](/img/2024/son/end.jpg "end")


# 软解流程

刚好身边有一台软解的switch，打算验证一遍网上所学

* sd卡格式化fat32格式
* 放上整合包（1.7.0）+switch 18.0.0 固件版本
* switch装上短接器
* ssd卡放入switch
* 先按住音量加不放手再按一下电源键进入黑屏rcm模式
* 通过usb线，使用TegraRcmGUI_v2.6注入（hekate v6.1.1 & Nyx v1.6.1），
* 马上按住音量减不放手就可以进入RCM模式

看到启动进入Hekate界面，就算成功。进入Hekate界面后松开音量减。

下周进行操作，希望和预期一致。

[参考文章](https://www.2023game.com/nsaita/pojie/205278.html)


# 备注

* [1：任天堂Switch NS 破解硬破装树莓派芯片 游戏机
](https://www.youtube.com/watch?v=KLayLc8TI9c&ab_channel=%E4%BA%8C%E6%89%8B%E5%85%89%E5%9C%88)
* [2：Switch破解后做双系统教程大气层虚拟系统emuMMC](https://www.youtube.com/watch?v=u4vuDy2RI30&ab_channel=%E4%BA%8C%E6%89%8B%E5%85%89%E5%9C%88)

* [3：硬破后如何上传游戏](https://www.youtube.com/watch?v=v3jilUWPs20&ab_channel=%E4%BA%8C%E6%89%8B%E5%85%89%E5%9C%88)

* [4：商家文档](https://docs.qq.com/doc/DTkV1QUdGVFhMVHR3?u=6b808ad5aa794c9d8cd732746e4733da)
* [5：硬破后升级大气层和switch固件](https://zhuanlan.zhihu.com/p/627504313)
* [6：升级视频](https://www.youtube.com/watch?v=MB4jEhz84E4&t=204s&ab_channel=%E5%A5%BD%E7%89%A9%E6%80%AA%E5%92%96)
* [7：砖头修复](https://www.youtube.com/watch?v=t0Y342KsKtc&ab_channel=%E6%80%80%E6%97%A7%E6%B8%B8%E6%88%8F%E5%A4%A7%E5%8F%94%E5%A4%A7%E6%85%A7)
* [8：香港人救砖过程](https://www.youtube.com/watch?v=34VANDsiuqs&t=300s&ab_channel=carcaschoi)

* [9：Switch系统升级和大气层破解相关](https://songlin.me/2023/05/13/switch/)
* [10：软解流程](https://www.corys.fun/?p=125)
* [11:使用注入器開機方法](https://www.youtube.com/watch?v=veh_fkcZgqI&ab_channel=SeanChan)
* [老外软解过程，缺少底下电脑的操作](https://www.youtube.com/watch?v=7EsPvinHZKY&ab_channel=Nevercholt)
* [软解机器使用电脑通过TegraRcmGUI注入](https://www.bilibili.com/video/BV11r4y1K73i/?vd_source=2d2cd39fe7b6ad8e2806cdd37537049e)
*[Tinfoil 黑商店](https://www.youtube.com/watch?v=o9qUgJtO8u8&t=2s&ab_channel=ThaGle%C7%9Dsh)
* [软解step by step](https://www.youtube.com/watch?v=OvZhFX183xg&t=10s&ab_channel=Manito)
* [详细过程，软解](https://www.youtube.com/watch?v=GfZXbCLVFB8&t=238s&ab_channel=Kristofer)
