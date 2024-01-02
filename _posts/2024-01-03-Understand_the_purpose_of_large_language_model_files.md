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

## tokenizer_checklist.chk

用途其实和checklist.chk 一样，检查token的完整性。

	eeec4125e9c7560836b4873b6f8e3025  tokenizer.model


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

## tokenizer.model

tokenizer的具体模型参数,这是经过训练得到的二进制文件,不可读。

tokenizer.model 文件存储了对输入文本进行编码和解码所需的特定参数。

具体来说，tokenizer 负责将原始的自然语言文本转换成模型可以理解的数值形式——通常是 token ID 序列。这个过程中涉及分词（将文本分割成单词、子词或字符）、添加特殊标记（如起始符、结束符、padding 符号等），以及可能的词汇表查找或嵌入矩阵操作。

# llama-2-7b-hf

llama-2-7b是不支持transformers，基于Huggingface的transformers来进行模型的微调是业界主流。所以Meta同时也发布了**llama-2-7b-hf**的版本，方便业界进行微调。

hf，就是表示hggingface的格式，支持transformers，可以使用到很多transformer提供的特性，更加方便模型的开发。

原始格式LLama ->转为huggingface（HF）格式，可以很方便使用工具进行就可以实现让llama-2-7b支持transformers，这个可以看我写的 [LLama2-7B Models Quantization Method](https://chenshake.com/2023/12/13/llama-conver-to-Hugging-Face/)

由于支持HF，模型的文件也发生不小变化。

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

safetensors 是一种安全快速存储和加载tensors的文件格式，safetensors 是 pickle 的一个安全替代方案，非常适合共享模型权重。

model-00001-of-00002.safesensors 模型权重参数分块1
model-00002-of-00002.safesensors，模型权重参数分块2

## model.safetensors.index.json

safesensors模型参数文件索引和描述模型切片的 JSON 文件。这个文件很长，我只截取了前面的几行。后面有模型分层的信息，32层。

* 模型切片的总数
* 每个切片的元数据,如名称、偏移地址、文件大小等
* 切片如何组合起来重新组成完整模型的说明
* 一些额外的模型信息,如模型名称、框架版本等元数据

3个文件是一组的。

* model.safetensors.index.json
* model-00001-of-00002.safesensors
* model-00002-of-00002.safesensors

	{
	  "metadata": {
		"total_size": 13476839424
	  },
	  "weight_map": {
		"lm_head.weight": "model-00002-of-00002.safetensors",
		"model.embed_tokens.weight": "model-00001-of-00002.safetensors",
		
## pytorch文件格式

* pytorch_model-00001-of-00002.bin
* pytorch_model-00002-of-00002.bin

pytorch文件的用途，和safetensors文件用途一样，就是安全性不同，所以现在其实已经很少使用pytorch格式

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
	
## special_tokens_map.json

tokenizer中特殊标记符(special tokens)到其对应的数字id的映射。可以打开查看。

包含 Tokenizer 特殊标记符（Special Tokens）到其对应的数字ID的映射。

一些常见的特殊标记符定义包括:

unk_token - 未登录词(out-of-vocabulary words)的标记id
sep_token - 句子分隔的标记id
pad_token - 填充序列到相等长度时使用的填充标记id
cls_token - 分类任务中使用的分类标记id
mask_token - 掩码语言模型任务中使用的掩码标记id


## tokenizer.json

tokenizer的配置信息,如字典大小,tokenize的策略等。

在Hugging Face的Transformers库中，`tokenizer.json`文件是用于存储预训练模型tokenizer的配置信息的。对于Llama-2-7b-hf这样的模型，其`tokenizer.json`文件通常包含以下信息：

1. **词汇表（Vocabulary）**：定义了每个token（包括子词、单词或特殊符号）与唯一ID之间的映射关系。
2. **分词规则（Tokenization Rules）**：描述了如何将原始文本分割成tokens的策略和参数，比如对于BERT这样的基于Byte-Pair-Encoding (BPE) 的模型，会记录合并的字符对规则。
3. **特殊令牌（Special Tokens）**：如起始符 `[CLS]`、结束符 `[SEP]`、填充符 `[PAD]` 以及其他自定义的特殊令牌及其对应的ID。
4. **其他配置参数**：可能还包括最大序列长度、是否进行lowercasing处理、是否保留标点符号等。

因此，`tokenizer.json` 文件对于正确设置和使用tokenizer至关重要，使得模型能够理解和生成符合其训练格式的输入输出数据。

## tokenizer.model

tokenizer，分词器，,这是经过训练得到的二进制文件,不可读。

## tokenizer_config.json

是用于配置和初始化模型的分词器（tokenizer）的配置文件。


# llama-2-7b-hf加载过程

![7B-hf-load](/img/2024/huggingface/7b-hf-load.jpg "7B-hf-load")

下载过程，发现有Downloading shards。

在大型语言模型中，由于文件大小通常非常大（几十GB甚至上百GB），为了方便管理和下载，这些模型的权重文件经常会被分割成多个小块或碎片（称为shards）。当加载模型时，如果配置正确且工具支持自动分片下载，它会根据需要去下载并整合这些shards。

当你看到"Downloading shards:"这样的提示时，意味着你的工具正在按照模型配置信息从服务器上获取相应的shard文件，并将它们拼接起来以完整加载模型。这种方式有助于在网络连接不稳定或者带宽有限的情况下更稳定地下载和使用大型模型。

例如，在Hugging Face Transformers库中，对于非常大的模型，其模型权重会存储在多个文件， 文件中，每个文件代表一个shard。加载时，库会自动处理shard的下载与合并。
