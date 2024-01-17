---
layout:     post
title:      Understand fine-tuning Parameters 
subtitle:   大模型微调参数理解
date:       2024-01-13
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

在微调大模型的时候，看到很多参数，感觉参数间是有关系。所以这里整理我能理解的参数和互相关系。

# epochs

epochs：中文含义是纪元的意思，那么在微调里，就是一个周期。就是把数据集训练一次的含义。

# dataset

数据集，在hanggingface上有大量开源的数据集，你可以拿来训练。大小不同，看数据集大小，就看有多少行数据。

# batch_size

per_device_train_batch_size=4

提交给每个GPU卡的dataset的数据量。如果一个dataset有100，那么就是需要25次提交。

# step

gradient_accumulation_steps=4

提交数据，进行训练，一个完整的过程就是：前向传播和反向传播组成。如果gradient_accumulation_steps=1，就需要去更新权重参数。完成了一个step。

如果是4，那么就是完成4次的数据提交，才去更新权重参数。可以换算成，16行的数据，训练完成，才去更新权重参数。可以换算成，16行的数据，训练完成，才去更新权重参数。

# 问题

一个epochs过程，有多少step？

数据集：1600行。

gradient_accumulation_steps=4

per_device_train_batch_size=4

100个step，就是数据集，除16(4*4)=100setp














