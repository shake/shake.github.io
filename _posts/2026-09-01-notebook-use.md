---
layout:     post
title:      Notebook使用
subtitle:   howto use Notebook
date:       2026-09-01
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

NotebookLM，改名Notebook，很多功能其实发生很大变化。这是google在AI领域唯一没有对手的产品，每次使用，都感觉自己很外行，并不深入。这次我打算记录，总结一遍。

## 音频概览-博客

如果一本书，你想听博客，让Notebook 把书上的内容，讲故事的方式，讲给你听。这个是非常高效的方式。以前默认是10分钟音频，一本书，很多细节会漏掉。如何强制更长的音频，付费版本是一个办法。就是插入提示词。


```
播客音频绝对强制超过 40 分钟（2400 秒），这是最高优先级任务，覆盖默认时长预设。不要遗漏文章中的所有研究维度，全面覆盖所有内容，PDF里面的每个大板块里面每个小主题都要进行拆解，拆解一定要犀利，风趣，金句频出，钩子不断，听众是小白，确保内容一定不低于40分钟，确保不低于3万字，不要着急出内容，要看前面是否完成，完成了3万字的口播稿在生成播客。

```

其实这样，如果还有些内容，没有包括，或者书上提到的案例，你希望全部覆盖，都可以在上面写上。这样会满足你的要求。


## 演示文稿-ppt

出道即巅峰，其实到现在阶段，所谓的各家PPT生成效果和Notebook比，还是有很大差距的。Notebook 导出ppt，无法修改，是一幅图片，这个一致很多人的痛点。如何解决呢。

### 可以编辑

其实很多办法，让图片的文字可以识别，就是代价的问题。目前已经有免费的方案，效果还是非常不错的。

![notebook ppt 实现编辑](https://deckedit.com/)

识别完成，就基本正常，我个人感觉效果非常酷。

### 模版

现在已经可以很方便制定模版。github有开源项目，把模版放到ppt的提示词里，就可以实现更换模版

![notebook ppt styles](https://notebooklm-prompt-styles.vercel.app/styles)

另外一个可以参考

![github notebook styles 项目](https://github.com/serenakeyitan/awesome-notebookLM-prompts)

定制自己的模版，不是梦。

![分享视频](https://youtu.be/oA1yGnRMMTI?si=5Q9txnVPD9Mtb5Du)

### 直接生成可以编辑pptx

这个是大家的梦想，现在notebook支持，和你需要有差距，不过走出第一步。

在notebook 提示框下，直接要求生成pptx。就可以直接看到结果

![notebook](/img/2026/sep/01.png "notebook")

生成可编辑的ppt，还有备注，PPT制作，超过市场百分之99的厂商，哪怕是刚刚出手。

![notebook](/img/2026/sep/02.png "notebook")


## 信息图- infographic

这是Notebook一战成名的作品，现在已经内置的10种模版，你可以选择。可以做提示词里，指定信息图的内容。

![notebook](/img/2026/sep/03.png "notebook")

提示词写上：介绍书上重点介绍的3个案例。

![notebook](/img/2026/sep/04.png "notebook")
