---
layout:     post
title:      Run Llama 2 On Local With Simple Way
subtitle:   Llama-2 安装指南
date:       2023-11-30
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

一直很纠结一个问题，在大模型那么流行的时候，我能干啥呢？如何能混入到大模型的圈子里？从哪里开始学习和了解大模型，适合我这种不会编程，不会算法人参与呢？

最后朋友给了一个思路，通过学习大模型的微调，可以让你实现大模型的入门。那么我就好好练习一把。

由于我没有专门的GPU，只能使用笔记本，用cpu来模拟GPU。所以需要专门选择支持cpu的大模型。笔记本的配置，建议16G内存。

选择了开源大模型：Llama-2 来测试学习。

# LM Studio

安装和配置Llama 2，有很多方式，这次采用最简单的方式来快速搭建，基本可以理解是一键安装。

你可以在windows ，apple，linux下安装Llama。

访问 [LM Studio](https://lmstudio.ai/) 

下载相应的版本安装就可以，安装过程，没任何选项，就安装完毕。打开LM Studio。

![LM Studio](/img/2023/llama/lmstudio.jpg "llmstudio")

默认你是可以直接通过搜索框去搜索你需要的大模型，不过因为在国内，导致无法搜索，哪怕有梯子，好像也不行。可以手工去huggingface 官网进行下载，把下载回来的大模型放到相应的目录就可以。

左边菜单的文件夹，其实就是存放大模型的地方，默认是在C盘，建议修改一下存储位置，这样不会导致系统盘塞满。大模型最小的规格都是3G以上。

![大模型](/img/2023/llama/folder.jpg "my models")

存放路径有要求，参考我存放模型的路径。用户名是huggingface的用户名。


`/path/to/models_folder/用户名/项目名/ModelName
D:\models_folder\chenshake\chinese\Chinese-Llama-2-7b-ggml-q4.bin
D:\models_folder\chenshake\en\llama-2-7b.ggmlv3.q4_0.bin`



## 注册账号

你需要登录大模型的github，huggingface [官方网站](https://huggingface.co) 注册一个账号。

## 下载

[下载地址](https://huggingface.co/TheBloke/Llama-2-7B-GGML) 页面里很多不同参数的版本下载，找到 llama-2-7b.ggmlv3.q4_0.bin，选择这个版本下载，可以降低对本地cpu和内存的要求。

[中文Llama-2大模型](https://huggingface.co/soulteary/Chinese-Llama-2-7b-ggml-q4/tree/main)

该文件的名称可以分解如下：

* llama-2-7b：表示模型的名称和参数量。
* ggmlv3：表示模型所使用的 GGML 库的版本。
* q4_0：表示模型的量化方法。

具体来说，q4_0 表示该模型使用了 4 位量化方法。量化方法是一种减少模型大小和计算成本的方法，它通过将模型参数的值近似为较小的整数来实现。

Llama-2-7B 是一个通用的语言模型，它可以用于各种任务，包括：

* 生成文本
* 翻译语言
* 回答问题
* 写不同类型的创意内容

该文件可以用于在各种设备上部署 Llama-2-7B。

# 开始使用

直接选择左边的AI chat的菜单，就可以操作。选择你需要使用的大模型，就可以开始进行对话。由于7B，使用CPU，中文版本很慢，英文的速度还是可以的。

启动一个 New Chat

![AI chat](/img/2023/llama/chat.jpg "AI chat")

可以看到需要使用4G内存。






