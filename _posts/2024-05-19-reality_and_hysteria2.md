---
layout:     post
title:      XTLS Reality And Hysteria2
subtitle:   手工安装XTLS Reality和Hysteria2
date:       2024-05-19
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - 技术
---

世界变化很快，2年没怎么关注，发现已经完全变化，都基本看不懂各种的梯子术语。不过也确实有创新，现在已经不需要依赖域名，443端口，那么其实就不用担心给封端口。

我是使用RackNerd的vps，一年12美刀，比我搬瓦工便宜不少，也可以好好对比一下，12美刀的vps，是否可以满足需求。

现在搭建变的很简单，到处都是一键安装脚本，有时候也不得不小心一点，同时也自己多学一点，这次安装都是直接使用官方的脚本加上手工配置，真的花心思读了一下官方文档。服务器端和windows的客户端，目前使用v2rayN

两个协议共同特点：

* 都是无需域名，
* 无需证书，
* 可以使用任意端口，
* 不支持套CDN
* 目前最安全的协议
* Reality+BBR3算法 和 Hysteria2+Brutal算法

# 使用分享

目前通过中国电信，中国移动线路，两种协议运行都没啥问题。

# XTLS Reality

[安装过程](https://github.com/shake/Xray-install)

# Hysteria2

[安装过程](https://github.com/shake/hysteria)

# BBR3

BBR3是为了加速，主要是给Reality使用。
手工安装，没有成功。主要原因是swap分区太小，导致无法启动，修改swap分区。

[BBR3安装](https://github.com/shake/xray-install/)

有空好好研究一下，为啥手工安装，导致无法启动。

#### Example

<details open>
<summary>Shopping list</summary>
<pre>
* Vegetables
* Fruits
* Fish
</pre>
</details>

