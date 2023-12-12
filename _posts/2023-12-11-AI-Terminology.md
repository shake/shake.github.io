---
layout:     post
title:      AI-Terminology
subtitle:   AI专业术语
date:       2023-12-11
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

专门用一篇blog来记录遇到新的AI专业术语，这些术语有时候含义，不是那么容易查询。

# GPTQ 

stands for “Generative Pre-trained Transformer Quantization”.
表示这个模型是支持GPU

# GGML 

GGML is a Tensor library for machine learning, it is just a C++ library that allows you to run LLMs on just the CPU or CPU + GPU. It defines a binary format for distributing large language models (LLMs). GGML makes use of a technique called quantization that allows for large language models to run on consumer hardware.

GPT-Generated Model Language，表示该模型支持CPU，目前已经淘汰。

# GGUF 

GPT-Generated Unified Format，这是最新的版本支持CPU的大模型，替代GGML。

# quantization

量化

 GGML supports a number of different quantization strategies (e.g. 4-bit, 5-bit, and 8-bit quantization), each of which offers different trade-offs between efficiency and performance.
 

#  Retrieval-augmented generation (RAG)

检索增强生成，意思是生成式问答（Generative Question Answering.）

# pretrained models and fine-tuned models 

预训练模型和微调模型