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

![原理图](/img/2024/art/flux/all.png "原理图")

一个模型，可以理解分为3部分

* CLIP
* VAE
* U-net


## 文生图

搭建一个文生图的工作流，其实折腾了不少时间，主要还是在处理中文和英文的切换，找组件。现在找到一个工作流合集，我打算一个一个分析，整理。 从最简单的开始。

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