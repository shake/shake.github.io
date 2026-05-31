---
layout:     post
title:      3个月AI学习总结
subtitle:   Summary of AI study 
date:       2026-05-16
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

三月份，受到OpenClaw的影响，决定全部时间投入去搞OpenClaw，搞清楚这个AI agent怎么玩。没想到。这3个月经历，有点魔幻。

深入投入去折腾的东西，真不少。确实很多项目的相通的。都是差不多。

* N8N
* Claude code
* CodeX
* OpenClaw
* Hermes agent
* Antigravity 2.0


## N8N

以前有报名一个自动化工具班，学习make，后面的N8N，就没有跟上。这次利用这次，把N8N补上。

理解的课程里的几个视频，1年前的n8n课程，今天看来，有些已经过时。不错对我来说，还是有收获，毕竟自己动手，实现解决自己的问题的工作流。

日常我经常把youtube的视频，直接发给gemini分析，看看总结就可以。几十分钟的视频内容，很难有那么多时间去看。慢慢就基本集中到几个博主上，订阅这几个博主的视频更新，发给Gemini 总结就可以。

Gemini这次5月份发布，至少看到的一个明显变化，分析youtube的视频，基本是秒杀。不需要任何的时间等待。我通过n8n的工作流。获取频道的更新，视频链接，利用api，发送链接给gemini lite分析，效果还是不错。

这个基本是gemini的免费额度就够用。你一天分析10个视频，对gemini来说，好像不是什么事情。

目前已经可以实现多个频道合并，24小时内更新的内容，早上九点发给我，一个视频，一封邮件。格式非常漂亮。这次自己对自己好一点。

手头上有minimax的token plan，支持生图。刚好验证了一下。搞了一个n8n工作流，各种风格，生图。效果还行。

我折腾过N8N 生视频，成本有点不可能承受。1秒的视频，少的0.1美元，贵的，1美元。基本很难支撑。我充值10美金，选择最便宜的视频模型，仅仅完成工作流验证，10美金就没了。

## Claude code

这个过去1年，技术圈非常狂热。一个命令行的东西，居然能那么时髦。确实是很难得。导致很多不会编程的用户，都开始用vscode。可见狂热程度。

我其实比较取巧，开始折腾的时候，已经支持第三方的api，我使用Minimax的 token plan，来验证Claude code的使用。

用claude code，写代码。折腾写文章。幸好自己3年前，开始折腾Mrakdown，所以对这些MD，没啥障碍。

用claude code，感觉最有意义，实际价值，估计是配置OpenClaw，优化OpenClaw，把OpenClaw，配置的更加安全，所有的api key，都存放在env，不放在配置文件。这样的苦活，累活，交给Claude code来完成。

其实在这方面，我没有感觉minimax的模型能力，比claude差多少。我没怎么用过Claude。仅仅是感觉minimax，可以满足我的要求，听懂我指令，去干活。

## OpenClaw

这个其实从安装开始折腾，WSL2，装了无数遍。非常熟悉。对接飞书和telegram。其实熟悉了，就是skill的使用。

OpenClaw，确实当时也是给人眼前一亮。我还自己开发的了一个skill，是使用Codex，开发了一个epub格式转换pdf。体验了一下skill开发，是这么简单。最后codex，顺利把这个skill代码推送到github。

确实感受到，让模型，agent干活的快乐。以前很多事情，不熟练，每次操作，都需要查，现在好了。让agent干活就可以。

外面所谓的著名的10大agent 开公司的配置玩法，我也是对着文档跑了一遍。才知道吹牛的，有多么夸张。学到不少东西，看到的都是浮夸。

我是对接Discord，账号三天都搞不定，最后还是咸鱼，5块钱解决。人生第一次花钱买账号。账号需要养。

最终我用OpenClaw，就是上传公众号，套用格式。这个工作，skill的活。Claude code，OpenClaw，Hermes agent，都可以做。

## Hermes agent

其实这个就没啥特别，和OpenClaw，基本一样的。很多人吹的所谓自我进化，都是经不起推敲。不给也是有不错的地方。我尝试hermes 配置 hermes，发现bug，去github提交bug，提交成功，并且项目负责人，给了一个高优先级。

hermes agent的神秘感其实没太强，配置记忆，经历的OpenClaw，这些都简单。

尝试通过Claude code，进行OpenClaw，Hermes agent的安装和配置。这也是没有问题。基本可以安装要求完成，并且可以形成文档。有时间，反复多次。就差不多。真的可以做到agent装agent。

## CodeX

因为有订阅，但是家人使用，我很少折腾，最近一个月，Codex，非常火爆。我也是顺便折腾一下，非常惊艳。其实就是昨天开始折腾。

装上Hyperframes skill，直接可以生成视频。甚至都没有成本。这个对我来说，非常震撼。对于折腾了很久n8n的视频的人来说，是一个很大的变化。

昨天把第一个Hyperframes的视频，上传公众号，还获得了不错的流量。也算是找到一个视频制作的突破口。

## Open Design

其实我也是打算折腾一下，实现做网页自由。可惜，不知道是minimax的问题，还是我的问题，我折腾几次，都没成功。后续慢慢折腾吧。

## Antigravity 2.0

这个其实是google出品，我也今天才装起来。折腾，其实是为了看对Hyperframe的表现如何。这点切实和OpenAI有差距。

## Image-2

这个也是我过去3个月遇到比较震撼的，生图。确实是保持了图片的一致性。修复旧照片，我已经无法看出区别了。

