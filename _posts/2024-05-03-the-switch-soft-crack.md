---
layout:     post
title:      switch 软破
subtitle:   Switch soft crack
date:       2024-05-03
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - 生活
---

上面整理的switch破解文档比较长，这里专门针对软破的机器的操作过程，做一个记录，方便日后使用。


下面就是操作的过程。

# SD卡操作

## 目录结构

**SD卡根目录**

![dir](/img/2024/son/dir.jpg "sd卡目录")


## sd卡格式化

直接把SD卡格式化，exFat格式，128k大小。

## 整合包

下载厂商提供的整合包，直接解压到SD卡根目录。

大气层的版本是向下兼容。最新的版本是:1.7.0,可以支持switch固件是18.0 和17.01底下的版本。

## switch固件

switch固件，是为了升级虚拟系统的switch版本，支持更多的游戏。

17.01，18.00，这两个版本的固件。

# switch操作

* 把sd卡插入switch机器，
* switch装上短接器。
* 先按住音量**加**不放手再按一下 电源键，进入黑屏rcm模式
* 通过usb线，使用TegraRcmGUI_v2.6注入（hekate v6.1.1 & Nyx v1.6.1）

淘宝购买的注入器，会因为版本太老，导致无法注入到新版本。有两个选项
* 通过usb线，升级注入器里的注入文件 hekate_ctcaer_6.1.1.bin
* 使用软解的注入器：TegraRcmGUI_v2.6


hekate_ctcaer_6.1.1.bin，通过[hekate v6.1.1 & Nyx v1.6.1](https://github.com/CTCaer/hekate/releases) 下载zip获得。

![注入](/img/2024/son/insert.jpg "注入")

成功后，你就可以看到hekate界面。

## 创建emuMMC

这个过程，其实是复制swith本体的固件版本，创建一个破解的环境运行。创建完成后，就可以直接进入虚拟系统。

登录虚拟系统的switch后，使用相册下的工具：daybreak，找到放到SD卡里的switch固件，直接升级虚拟系统的固件版本，这个不会影响到switch的本体的固件。

## 装游戏

通过DBI的方式安装软解，不需要拔卡，非常方便。

# 使用注意

正常就直接进入虚拟系统使用，休眠，重启都没问题。如果进入的switch本体系统，你还是需要关机，重新操作注入的流程，才能进入虚拟系统。





