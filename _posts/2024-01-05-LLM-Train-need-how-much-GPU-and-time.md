---
layout:     post
title:      How Many GPU Need For LLM Training
subtitle:   大模型算力计算
date:       2024-01-06
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

大模型要什么了解，最好的入门，就是计算训练一次大模型，需要多少张卡，多少时间。这个过程中，真的接触很多新的词汇。



**参考文章**

* [大模型训练为什么用 A100 不用 4090
](https://mp.weixin.qq.com/s/PCbvJdIKGXDugUt1aul5Cg)
* [语言模型的训练时间：从估算到 FLOPs 推导
](https://zhuanlan.zhihu.com/p/646905171)
* [大模型算力推演优化实战
](https://mp.weixin.qq.com/s/oUe_Vw0vfMvXJ-w97dkK4w)
* [OpenAI 总监演讲](https://36kr.com/p/2278602196457221)

深刻理解上面材料的内容，总结。




# FLOPS

FLOPS（floating-point operations per second），用于测量计算机在单位时间内可以执行的浮点运算次数。我第一次见到这个单词，估计就一年前。一直也没搞懂具体的含义。


# 单位

计算FLOPS的单位，比较吓人。

*1M (MegaFLOPS) = 10^6 FLOPS
*1G (GigaFLOPS) = 10^9 FLOPS
*1T (TeraFLOPS) = 10^12 FLOPS
*1P (PetaFLOPS) = 10^15 FLOPS
*1E (ExaFLOPS) = 10^18 FLOPS
*1Z (ZettaFLOPS) = 10^21 FLOPS

还要数学换算字母

模型的参数，目前都是用B来计算

1B（billion）=10^9


# 结论

推算半天，发现很难自圆其说。主要原因有3个

* 稀疏算力和稠密算力，这个外面的计算，都是采用稀疏算力。实际大模型训练都是采用稠密算力。
* 混合算力的比例，这块很多地方都不考虑。
* GPU卡利用率，这个是一个不确定的数值，可以调整，满足公开的数据，使得结果一致。



























