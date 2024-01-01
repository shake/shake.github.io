---
layout:     post
title:      Transformers 学习
subtitle:   Transformers入门
date:       2023-12-31
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

下面的内容都是来自各个学习的视频，我针对视频，整理我的理解。非常感谢这些博主的分享，我安装Llama 2一个多月，一堆问题，都是需要慢慢的解决和理解.下面是我做的笔记。

[你可是处女座啊](https://www.youtube.com/watch?v=Xeu3qFTP9qY&list=PL2ecZnqc6-L7r8tSr6r3bYHsqkyChUVbM&index=2) 

# Transformers

## 常见自然语言处理任务

* 情感分析(sentiment-analysis):对给定的文本分析其情感极性
* 文本生成 (text-generation) : 根据给定的文本进行生成
* 命名实体识别(ner):标记句子中的实体
* 阅读理解 (question-answering): 给定上下文与问题，从上下文中抽取答案
* 掩码填充 (fill-mask) : 填充给定文本中的掩码词
* 文本摘要(summarization):生成一段长文本的摘要
* 机器翻译 (translation) : 将文本翻译成另一种语言
* 特征提取 (feature-extraction):生成给定文本的张量表示
* 对话机器人 (conversional) : 根据用户输入文本，产生回应，与用户对话

## 自然语言处理的几个阶段

**第一阶段:统计模型+数据(特征工程)**

	* 决策树、SVM、HMM、CRF、TF-IDF、BOW
	
**第二阶段:神经网络+数据**

	* Linear、CNN、RNN、GRU、LSTM、Transformer、Word2vec、Glove
	
**第三阶段: 神经网络 + 预训练模型 + (少量)数据**

	* GPT、BERT、ROBERTa、ALBERT、BART、T5
	
**第四阶段: 神经网络+更大的预训练模型 + Prompt**

	* ChatGPT、Bloom、LLaMA、Alpaca、Vicuna、MOSS、文心一言、通义千问、星火
	
## Transformers简单介绍

官方网址: https://huggingface.co

HuggingFace出品，当下最热、最常使用的自然语言处理工具包之一，不夸张的说甚至没有之一；

实现了大量的基于Transformer架构的主流预训练模型，不局限于自然语言处理模型，还包括图像、音频以及多模态的模型；

提供了海量的预训练模型与数据集，同时支持用户自行传，社区完善，文档全面，三两行代码便可快速实现模型训练推理，上手简单。

## Transformers及相关库

* Transformers: 核心库，模型加载、模型训练、流水线等
* Tokenizer:分词器，对数据进行预处理，文本到token序列的互相转换
* Datasets:数据集库，提供了数据集的加载、处理等方法
* Evaluate:评估函数，提供各种评价指标的计算函数
* PEFT:高效微调模型的库，提供了几种高效微调的方法，小参数量撬动大模型
* Accelerate: 分布式训练，提供了分布式训练解决方案，包括大模型的加载与推理解决方案
* Optimum:优化加速库，支持多种后端，如Onnxruntime、OpenVino等
* Gradio:可视化部署库，几行代码快速实现基于Web交互的算法演示系统

## Transformers安装

安装命令

	pip -q install transformers datasets evaluate peft accelerate gradio optimum sentencepiece
	pip -q install  scikit-learn pandas matplotlib tensorboard nltk rouge


## Transformers极简实例

文本分类

	# 文本分类
	# 导入gradio
	import gradio as gr
	# 导入transformers相关包
	from transformers import *
	# 通过Interface加载pipeline并启动文本分类服务
	gr.Interface.from_pipeline(pipeline("text-classification", model="uer/roberta-base-finetuned-dianping-chinese")).launch()
	
阅读理解

	# 导入gradio
	import gradio as gr
	# 导入transformers相关包
	from transformers import *
	# 通过Interface加载pipeline并启动阅读理解服务
	gr.Interface.from_pipeline(pipeline("question-answering", model="uer/roberta-base-chinese-extractive-qa")).launch()
	
# pipeline

## 什么是Pipeline

* 将数据预处理、模型调用、结果后处理三部分组装成的流水线
* 使我们能够直接输入文本便获得最终的答案

![pipeline](/img/2023/transformers/pl-00.jpg "流水线")

## pipeline 支持的任务类型

当前支持的任务集合包括：

文本相关任务：

| 文本任务                                   | 说明                                                                                                                    |
|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|
| text-classification   (sentiment-analysis) | 情感分析                                                                                                                |
| token-classification   (ner): 命名实体识别 | 命名实体识别                                                                                                            |
| question-answering                         | 自动问答                                                                                                                |
| fill-mask                                  | 填充被遮盖的词、片段                                                                                                    |
| summarization                              |  自动摘要                                                                                                               |
| translation                                | 机器翻译                                                                                                                |
| text2text-generation                       | 指转换文本，就像您将文本翻译成另一种语言                                                                                |
| text-generation                            | 文本生成                                                                                                                |
| conversational                             | 对话响应建模是根据提示生成相关、连贯且知识丰富的对话文本的任务。 这些模型在聊天机器人中得到应用，并作为语音助手的一部分 |
| table-question-answering                   | 表问答（Table QA）数据库查询                                                                                            |
| zero-shot-classification                   | 零训练样本分类                                                                                                          |


完整pipeline支持的任务内容

* audio-classification
* automatic-speech-recognition
* text-to-audio
* feature-extraction
* text-classification
* token-classification
* question-answering
* table-question-answering
* visual-question-answering
* document-question-answering
* fill-mask
* summarization
* translation
* text2text-generation
* text-generation
* zero-shot-classification
* zero-shot-image-classification
* zero-shot-audio-classification
* conversational
* image-classification
* image-segmentation
* image-to-text
* object-detection
* zero-shot-object-detection
* depth-estimation
* video-classification
* mask-generation
* image-to-image

## Pipeline创建与使用

**根据任务类型直接创建Pipeline**

	pipe = pipeline("text-classification")
	
**指定任务类型，再指定模型，创建基于指定模型的Pipeline**

	pipe = pipeline("text-classification", model="uer/roberta-base-finetuned-dianping-chinese")
	
**预先加载模型，再创建Pipeline**

	model = AutoModelForSequenceClassification.from_pretrained("uer/roberta-base-finetuned-dianpingchinese")
	tokenizer = AutoTokenizer.from_pretrained("uer/roberta-base-finetuned-dianping-chinese")pipe = pipeline("text-classification", model=model, tokenizer=tokenizer)
	
**使用GPU进行推理加速**

	pipe = pipeline("text-classification", model="uer/roberta-base-finetuned-dianping-chinese", device=0)
	

## Pipeline的背后实现

**Step1 初始化Tokenizer**

	tokenizer = AutoTokenizer.from pretrained("uer/roberta-base-finetuned-dianping-chinese")
	
**Step2 初始化Model**

	model = AutoModelForSequenceClassification.frompretrained("uer/roberta-base-finetuned-dianping-chinese"

**step3 数据预处理**

	input text ="我觉得不太行!"
	inputs = tokenizer(input text, return tensors="pt")
	
**Step4 模型预测**

	res =model(**inputs).logits
	
**Step5结果后处理**

	pred = torch.argmax(torch.softmax(logits, dim=-1)).item()
	result = model.config.id2label.get(pred)