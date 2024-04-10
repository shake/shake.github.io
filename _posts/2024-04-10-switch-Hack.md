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

我不玩游戏，主要是给孩子做一个记录。

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

逛淘宝，会发现所谓软破和硬破，对于2024年，硬破是唯一选择，并且现在的硬破，采用树莓派的方案，拆开主机焊接破解芯片的方式破解，费用已经降低到150元以下，包括OLED的硬破，以前因为OLED的硬破解方案，手工费会贵一点，现在收费都一样了。



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

## Ban 机

也就是被任天堂拉黑的机器，被 Ban 后的机器无法与任天堂官方服务器联网，除此外和未 Ban 的机器无任何区别，判断是否 Ban 机也很简单，在正版系统下进入 eshop 看看是否能加载出来就知道了。

如果你不想被 Ban，后续玩破解游戏都需要在 虚拟系统 进行，在虚拟模式下进行的所有操作都是跟 NS 主机的原版系统 完全独立 的，所以只要你不作死和手贱，一般就不会被 Ban。

# 特斯拉和金手指

打游戏没法通关，采用作弊的方式。我家小孩不喜欢这样玩，所以就不需要研究这块。

# 上传游戏

需要一台windows电脑和一根Type-c的数据线。

要记住，内存卡任何文件不能出现中文

1：下载好游戏的安装包，打开switch相册

![相册](/img/2024/son/pic.jpg "相册")

2：点击DBI插件

![DBI](/img/2024/son/dbi.jpg "DBI")

3：点击右手柄x按键

![x](/img/2024/son/x.jpg "X")

4:switch 用数据线连接电脑，电脑会出现弹窗，或者出现switch图标。

![install](/img/2024/son/install.jpg "install")

5：将游戏本体，更新包，复制到五号盘（Micro SD install）

一定要记住**只copy文件，不能是文件夹**

等待进度条读取完成，按下右手柄**房子键**

![home](/img/2024/son/home.png "home")

# 删除软件

找到要删除的游戏，点击手柄上的 + 号，最后点击删除软件即可

选择：数据管理，选择删除软件就可以。

![delete](/img/2024/son/delete.jpg "delete")

# 系统版本和大气层版本

![version](/img/2024/son/version.jpg "version")

# 游戏不能玩

![work](/img/2024/son/work.jpg "work")

# 更换内存卡

现在是一张512G的内存卡，家里还有一张1T的内存卡，如何把1T的卡也能用起来呢。

已经破解的系统，默认是从SD卡启动。

1. 格式化

电脑格式化SD卡为exfat格式，分配单元大小为32k。

![exfat](/img/2024/son/exfat.jpg "exfat")

2. switch 支持exfat驱动

让swich识别exfat格式的卡。

由于我的switch已经破解使用中，所以肯定是带驱动，可以正常识别。

这个步骤可以跳过

3. 大气层整合包

把大气层整合包解压，放到SD卡根目录下。

4. 开机启动

把SD卡插入switch，进入Hekate，选择虚拟系统

![vir](/img/2024/son/vir.jpg "vir")

5. 创建emuMMC

![emc](/img/2024/son/emc.jpg "emc")

6. SD卡文件

![sd](/img/2024/son/sd.jpg "sd")

7. 成功

![ok](/img/2024/son/ok.jpg "ok")

8. 检查

![open](/img/2024/son/open.jpg "open")

9. 启动

![test](/img/2024/son/test.jpg "test")

10. 选择大气层虚拟系统

![end](/img/2024/son/end.jpg "end")