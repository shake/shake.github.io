---
layout:     post
title:      LLama2-7B转HuggingFace
subtitle:   LLaMA2-7B convert to HF model
date:       2023-12-13
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

希望可以从零开始，一步一步进行模型的微调。发现第一步，就是需要拿llama 2的原始模型，进行格式的转换，国内外使用Huggingface模型格式和其配套的通用代码进行微调是主流。这里就记录格式转换全过程。


# 格式转换

将原始格式的LLaMA模型，也就是你从官方下载回来的代码，转换为Hugging Face Transformers支持的格式

## 原始模型准备

原始模型，官方下载回来的文件包括：

![llama-7B](/img/2023/modelscope/llama-7B.jpg "llama-7B")

你需要增加最后的两个文件，同时创建一个空目录**7B**，才能顺利转换为Hugging Face Transformers支持的格式:

![llama-7B](/img/2023/modelscope/llama-7B-add.jpg "llama-7B")


## conda

	conda create -n shake python=3.11
	conda activate shake

## 安装所需库

   首先确保你已经安装了`transformers`和`torch`库。


	pip install transformers torch



## 转换工具


	git clone https://github.com/huggingface/transformers

   
   安装转换工具的依赖
   
	pip install sentencepiece protobuf accelerate


## 创建输出目录

在**Llama-2-7b** 同级的目录下，创建空目录**Llama-2-7b-hf**

![目录结构](/img/2023/modelscope/dir.jpg "目录结构")

## 进行转换

运行下面命令，就可以获得HF格式。

	python3 transformers/src/transformers/models/llama/convert_llama_weights_to_hf.py --input_dir ./Llama2-7b --model_size 7B --output_dir ./Llama-2-7b-hf
	

**转换结果**：

![llama-7B-hf](/img/2023/modelscope/hf.jpg "llama-7B-hf")

# 转换优势

将原始格式的LLaMA模型转换为Hugging Face Transformers支持的格式有以下几个原因：

1. **统一性**：
   Hugging Face Transformers是一个流行的自然语言处理库，它提供了大量的预训练模型和易于使用的API。将LLaMA模型转换为这个库支持的格式可以使你的工作流程与其他使用Transformers的项目保持一致。

2. **兼容性**：
   转换后的模型可以无缝地与Hugging Face Transformers生态系统集成。这包括使用`AutoModel`、`Trainer`和其他高级工具来微调、评估和部署模型。

3. **易用性**：
   使用Hugging Face Transformers意味着你可以利用其丰富的功能集，如自动日志记录、多GPU支持、混合精度训练等，这些功能可以帮助你更高效地进行实验和开发工作。

4. **社区支持**：
   Hugging Face Transformers拥有一个庞大的开发者社区，这意味着你可以找到许多教程、代码示例和问题解答，这对于快速解决问题和学习新技术非常有帮助。

5. **可扩展性**：
   通过将LLaMA模型转换为Hugging Face Transformers格式，你可以轻松地将其与其他Transformers模型组合，比如在流水线（pipeline）中使用不同的模型组件。

6. **标准化**：
   Hugging Face Transformers提供了一种标准化的方式来加载和保存模型，这样其他人也可以更容易地复现你的工作或贡献代码。

总的来说，将LLaMA模型转换为Hugging Face Transformers格式可以让你充分利用这个强大的库，并使你的研究更具可重复性和可扩展性。

# 备注

	git clone https://www.modelscope.cn/angelala00/Llama-2-7b.git

