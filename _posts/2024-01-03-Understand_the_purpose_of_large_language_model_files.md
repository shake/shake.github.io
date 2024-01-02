---
layout:     post
title:      Understand the purpose of large language model files
subtitle:   理解大语言模型文件用途
date:       2024-01-03
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

日常访问huggingface的大模型，基本都是可以看到一堆文件，具体的用途，其实是一直困惑我的问题。今天整理一下。

需要致谢一下这位博主：[01coder](https://www.youtube.com/watch?v=eHZRt7DNuts&list=PL2fGiugrNoohh4jfFxfMvcK1ktfLyM41a) 分享。

# 模型文件介绍

今天我们就以模型 [meta-llama/Llama-2-7b](https://huggingface.co/meta-llama/Llama-2-7b/tree/main) 和 [meta-llama/Llama-2-7b-hf](https://huggingface.co/meta-llama/Llama-2-7b-hf/tree/main) 做一个说明。

# llama-2-7b

![7B](/img/2024/huggingface/7b.jpg "7B")

## checklist.chk

在 Meta 的 LLaMA 模型中，checklist.chk 文件对于确保 LLaMA 模型的正确性。

打开checklist.chk，其实就是2个文件的MD5数值。确保下载和加载的完整性。

	daa8e3109935070df7fe8fc42d34525e  consolidated.00.pth
	9a3757de7196d1840b551a85b82efbc8  params.json


需要注意的是meta-llama/Llama-2-7b-hf，是没有这个文件的。

## consolidated.00.pth

保存模型权重的文件，使用使用Python的 pickle工具将数据序列化到一个文件中。

权重文件会具体记录模型中每个层（包括但不限于自注意力层、全连接层等）的所有权重和偏置。对于Transformer架构的语言模型，这些参数通常包括：

1. **词嵌入矩阵**：用于将输入词汇映射到高维向量空间。
2. **位置编码**：表示输入序列中各个位置的上下文信息。
3. **自注意力机制的权重矩阵**：包括查询（Query）、键（Key）和值（Value）对应的权重矩阵，用于计算自注意力得分。
4. **前馈神经网络（FFN）的权重**：这是Transformer内部除了自注意力之外的另一部分，通常由两层线性变换组成，中间可能有激活函数如ReLU。
5. **LayerNorm层的参数**：包括每一层的均值和标准差归一化的gamma和beta参数。
6. **分类器头部的权重**（如果适用）：在预训练阶段用于生成下一个词预测任务的输出层参数，在微调时可能会用作其他下游任务的输出层。

此外，权重文件不包含模型结构信息，但为了恢复模型并进行推理或继续训练，通常需要与之配套的模型架构描述文件或代码来解析加载这些权重。

## params.json

这个文件是可以打开访问

	{"dim": 4096, "multiple_of": 256, "n_heads": 32, "n_layers": 32, "norm_eps": 1e-05, "vocab_size": -1}
	
这些参数说明如下：

- `dim`: 表示模型的隐藏层维度，这里为4096，意味着模型中每个隐藏层（如Transformer中的隐藏状态或FFN内部）具有4096个特征维度。

- `multiple_of`: 这个参数通常表示某种结构的尺寸是另一个基础尺寸的倍数，在这里的值为256，可能是指某些维度或者子模块大小应为256的整数倍。

- `n_heads`: 表示模型中的自注意力机制（Multi-Head Attention）头部数量，这里为32，意味着在每一个自注意力层，模型会并行执行32个独立的注意力计算。

- `n_layers`: 指模型的总层数，这里是32，表明该模型有32个Transformer层（或者称为块、阶段等）。

- `norm_eps`: 这是LayerNorm层（Layer Normalization）中用于数值稳定性的极小正数阈值，防止除以零。这里的值1e-05是一个常见的设置，用于在计算均值和标准差时避免被零分母问题影响。

- `vocab_size`: 表示模型所使用的词汇表大小，这里的值为-1，表示没有定义词汇表大小。




