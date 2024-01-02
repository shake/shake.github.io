---
layout:     post
title:      Purpose of LLM Files
subtitle:   理解大语言模型Llama-2文件用途
date:       2024-01-03
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

日常访问huggingface的大模型，基本都是可以看到一堆文件，具体的用途，其实是一直困惑我的问题。今天整理一下。

需要致谢一下这位博主：[01coder](https://www.youtube.com/watch?v=eHZRt7DNuts&list=PL2fGiugrNoohh4jfFxfMvcK1ktfLyM41a) 分享。

[Tokenizer说明](https://medium.com/@vyperius117/understanding-the-llama2-tokenizer-working-with-the-tokenizer-locally-using-transformers-2e0f9e69d786)

# 模型文件介绍

今天我们就以模型 [meta-llama/Llama-2-7b](https://huggingface.co/meta-llama/Llama-2-7b/tree/main) 和 [meta-llama/Llama-2-7b-hf](https://huggingface.co/meta-llama/Llama-2-7b-hf/tree/main) 做一个说明。

# llama-2-7b

这是Facebook开源的原始模型文件。红框的5个文件，下面逐一介绍。

![7B](/img/2024/huggingface/7b.jpg "7B")

## checklist.chk

打开checklist.chk，其实就是2个文件的MD5数值。确保下载和加载的完整性。

	daa8e3109935070df7fe8fc42d34525e  consolidated.00.pth
	9a3757de7196d1840b551a85b82efbc8  params.json


需要注意的是meta-llama/Llama-2-7b-hf，是没有这个文件的。

## tokenizer_checklist.chk

用途其实和checklist.chk 一样，检查token的完整性。

	eeec4125e9c7560836b4873b6f8e3025  tokenizer.model


## consolidated.00.pth

模型权重的文件，使用使用Python的 pickle工具将数据序列化到一个文件中。文件边上有一个pikle的标识。huggingface不建议使用这样的文件格式，出于安全的原因。

权重文件会具体记录模型中每个层（包括但不限于自注意力层、全连接层等）的所有权重和偏置。对于Transformer架构的语言模型，这些参数通常包括：

1. **词嵌入矩阵**：用于将输入词汇映射到高维向量空间。
2. **位置编码**：表示输入序列中各个位置的上下文信息。
3. **自注意力机制的权重矩阵**：包括查询（Query）、键（Key）和值（Value）对应的权重矩阵，用于计算自注意力得分。
4. **前馈神经网络（FFN）的权重**：这是Transformer内部除了自注意力之外的另一部分，通常由两层线性变换组成，中间可能有激活函数如ReLU。
5. **LayerNorm层的参数**：包括每一层的均值和标准差归一化的gamma和beta参数。
6. **分类器头部的权重**（如果适用）：在预训练阶段用于生成下一个词预测任务的输出层参数，在微调时可能会用作其他下游任务的输出层。


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

## tokenizer.model

tokenizer就理解成分词器，通过tokenizer对文本进行分词。

# llama-2-7b-hf

llama-2-7b是不支持transformers，基于Huggingface的transformers来进行模型的微调是业界主流。所以Meta同时也发布了**llama-2-7b-hf**的版本，方便业界进行微调。

hf，就是表示hggingface的格式，支持transformers，可以使用到很多transformer提供的特性，更加方便模型的开发。

原始格式LLama ->转为huggingface（HF）格式，可以很方便使用工具进行就可以实现让llama-2-7b支持transformers，这个可以看我写的 [LLama2-7B Models Quantization Method](https://chenshake.com/2023/12/13/llama-conver-to-Hugging-Face/)

这是Facebook开源的huggingface（HF）格式。一共有12个文件。逐一介绍。

![7B-hf](/img/2024/huggingface/7b-hf.jpg "7B-hf")

## config.json

模型架构的主要配置,如 Bert模型设置,预测头部设置,训练参数等。类似params.json

	{
	  "_name_or_path": "meta-llama/Llama-2-7b-hf",
	  "architectures": [
		"LlamaForCausalLM"
	  ],
	  "bos_token_id": 1,
	  "eos_token_id": 2,
	  "hidden_act": "silu",
	  "hidden_size": 4096,
	  "initializer_range": 0.02,
	  "intermediate_size": 11008,
	  "max_position_embeddings": 4096,
	  "model_type": "llama",
	  "num_attention_heads": 32,
	  "num_hidden_layers": 32,
	  "num_key_value_heads": 32,
	  "pretraining_tp": 1,
	  "rms_norm_eps": 1e-05,
	  "rope_scaling": null,
	  "tie_word_embeddings": false,
	  "torch_dtype": "float16",
	  "transformers_version": "4.31.0.dev0",
	  "use_cache": true,
	  "vocab_size": 32000
	}

## generation_config.json

文本生成相关的模型配置。

	{
	  "bos_token_id": 1,
	  "do_sample": true,
	  "eos_token_id": 2,
	  "pad_token_id": 0,
	  "temperature": 0.6,
	  "max_length": 4096,
	  "top_p": 0.9,
	  "transformers_version": "4.31.0.dev0"
	}

## safetensors文件格式

模型权重的文件。safetensors 是一种安全快速存储和加载tensors的文件格式，safetensors 是 pickle 的一个安全替代方案，非常适合共享模型权重。

* model-00001-of-00002.safesensors 模型权重参数分块1
* model-00002-of-00002.safesensors，模型权重参数分块2

## model.safetensors.index.json

safesensors模型参数文件索引和描述模型切片的 JSON 文件。这个文件很长，我只截取了前面的几行。后面有模型分层的信息，32层。

* 模型切片的总数
* 每个切片的元数据,如名称、偏移地址、文件大小等
* 切片如何组合起来重新组成完整模型的说明
* 一些额外的模型信息,如模型名称、框架版本等元数据

文件的简单内容：

	{
	  "metadata": {
		"total_size": 13476839424
	  },
	  "weight_map": {
		"lm_head.weight": "model-00002-of-00002.safetensors",
		"model.embed_tokens.weight": "model-00001-of-00002.safetensors",
		
safetensors2个模型权重文件，需要结合model.safetensors.index.json，进行加载。

## pytorch文件格式

模型权重的文件。

* pytorch_model-00001-of-00002.bin
* pytorch_model-00002-of-00002.bin

pytorch文件，你会发现文件列表上有一个pickle标记，表示是使用Python的 pickle工具将数据序列化。由于不安全的原因，现在已经是不推荐使用。

## pytorch_model.bin.index.json

pickle序列化的pytorch索引和描述模型切片的 JSON 文件。可以看到模型分为32层。

* 模型切片的总数
* 每个切片的元数据,如名称、偏移地址、文件大小等
* 切片如何组合起来重新组成完整模型的说明
* 一些额外的模型信息,如模型名称、框架版本等元数据

{
  "metadata": {
    "total_size": 13476839424
  },
  "weight_map": {
    "lm_head.weight": "pytorch_model-00002-of-00002.bin",
    "model.embed_tokens.weight": "pytorch_model-00001-of-00002.bin",
    "model.layers.0.input_layernorm.weight": "pytorch_model-00001-of-00002.bin",
	
pytorch的2个文件，需要结合pytorch_model.bin.index.json，进行加载。
	
## special_tokens_map.json

tokenizer中特殊标记符(special tokens)到其对应的数字id的映射。可以打开查看。

包含 Tokenizer 特殊标记符（Special Tokens）到其对应的数字ID的映射。

一些常见的特殊标记符定义包括:

* unk_token - 未登录词(out-of-vocabulary words)的标记id
* sep_token - 句子分隔的标记id
* pad_token - 填充序列到相等长度时使用的填充标记id
* cls_token - 分类任务中使用的分类标记id
* mask_token - 掩码语言模型任务中使用的掩码标记id


## tokenizer.json

分词器的词汇配置文件。

## tokenizer.model

模型的分词器，这是经过训练得到的二进制文件,不可读。

## tokenizer_config.json

分词器的配置文件

# llama-2-7b-hf加载过程

![7B-hf-load](/img/2024/huggingface/7b-hf-load.jpg "7B-hf-load")

下载过程，发现有Downloading shards。
在Hugging Face Transformers库中，对于非常大的模型，其模型权重会存储在多个文件， 每个文件代表一个shard。加载时，库会自动处理shard的下载与合并。
