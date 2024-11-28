---
layout:     post
title:      Flux 学习总结
subtitle:   Flux summarize
date:       2024-11-28
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - 技术

---

这几天搞Flux，不停的看视频，看看大家如何使用ComfyUI和Flux，很多工作流，是混合了Stable diffusion和Flux来完成相关的出图，这个其实也导致理解起来比较困难。我就专门找Flux的工作流来理解，这样可以更快了解Flux。

不同阶段，需要，希望了解的内容，是不一样。尤其学习深入后，对原理，也产生的好奇。下面这张原理图，应该算是目前国内唯一一个可以讲明白的。

## 原理图

下面基本我每次做一个工作流，基本都会复习一遍这个工作流的流程。理解**采样器** 和**潜在空间**

![原理图](/img/2024/art/flux/all.png "原理图")

一个生图模型，可以理解分为3部分

* CLIP
* VAE
* U-net


## 文生图

搭建一个文生图的工作流，其实折腾了不少时间，主要还是在处理中文和英文的切换，找组件。现在找到一个工作流合集，我打算一个一个分析，整理。 从最简单的开始。流程图是清晰版本，放到，其实是可以对着抄一遍。

提示词

```
astronaut walking on sunshine, vfx explosions in the background by john carpenter and michael bay, parting the sea, split the moon, summer breeze, big wave, tropical pacific, 80 and 90 psychedelic city pop sfx by haruomi hosono, tatsuro yamashita, shigeru suzuki, yura yura teikoku, shintaro sakamoto, s kitoyaka and omega tribe, makoto matsushita, toshiki kadomatsu
OverallDetailXL 

```

![文生图](/img/2024/art/flux/flux1.png "文生图")

对Flux来说，相同的提示词，基本都可以产生不错的效果。就是提示词，写作比较高深。 相信到最后，难度会降低到和Fooocus一样，简单，可控。


这个工作流，和我以前创建的工作流不同的地方：

* 把**潜在空间（K采样器）** 的参数拆开，可以配置的参数更多，增加了 **随机噪波**

我把整个工作流，存成模板，如果需要使用，通过 **节点预设** ，就可以把当前的工作流创建出来，后续在这个基础上进行优化。


## 图生图

图生图，其实就把**Latent** ，换成图片。

![图生图](/img/2024/art/flux/flux2.png "图生图")

这里面其实会涉及到参数的调整：降噪：从0.5到1.

上面有提示词和参考图片。

* 降噪设置为：1，参考图片的效果为0，等于文生图。 上面文生图的提示词，继续可以生成一个宇航员的照片。
* 降噪设置为：0.5, 生成的图片，基本就是原图的复制。
* 降噪设置为：0.6，到0.8, 生成的图片，会参考原图，进行重绘。

**重点**

* 默认的图生图工作流，是不能定义图片的大小，这个工作流，通过引入一个：**限制图片区域**， 实现输出图片的大小的调整。

* 图片输入，多了一个**VAE Encode** 。参考原理图。


## 文生图+Lora

工作流加**Lora**，是常态，这个Lora的选择，其实就完全靠经验，多个Lora可以实现串联，不过今天发现：用了Lora，可以大幅减少和降低提示词的难度。


![文生图+lora](/img/2024/art/flux/flux3.png "文生图+Lora")

一个很简单的提示词，通过**Trigger Words**，就可以实现非常不错的效果。

```
An Asian girl, with long black hair and a blue skirt, in the park.：LLL

```

