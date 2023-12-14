---
layout:     post
title:      LLama2-7B model convert 
subtitle:   LLaMA2-7B  HF GGUF and GPTQ
date:       2023-12-13
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

希望可以从零开始，一步一步进行模型的微调。发现第一步，就是需要拿llama 2的原始模型，进行格式的转换，国内外使用HuggingFace模型格式（简称HF格式）和其配套的通用代码进行微调是主流。这里就记录格式转换全过程。

[超详细LLama2+Lora微调实战
](https://mp.weixin.qq.com/s/KJTkatOrf9TqSrtBPZbKwA)

访问HuggingFace，很多模型提供GGML,GGUF格式和GPTQ格式，目前GGML格式已经淘汰，使用GGUF替代，其实这些大模型格式是这样进行转换：

* **原始格式LLama ->转为huggingface（HF）格式**
* **huggingface格式（HF） ->转为GGUF格式**
* **huggingface格式（HF） ->转为GPTQ格式**

原始格式LLama转换HF格式，是没有精度的损失，转换成GGUF和GPTQ，你可以设置参数，降低精度，缩小模型。

# HF转换

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

   首先确保你已经安装了`transformers`和`torch`库。需要确认是否使用GPU

### GPU

	pip install transformers torch

### CPU

	pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu



## 转换工具


	git clone https://github.com/huggingface/transformers

   
   安装转换工具的依赖
   
	pip install sentencepiece protobuf accelerate


## 创建输出目录

在**Llama-2-7b** 同级的目录下，创建空目录**Llama-2-7b-hf**

![目录结构](/img/2023/modelscope/dir.jpg "目录结构")

## 进行转换

运行下面命令，就可以获得HF格式。

	python3 transformers/src/transformers/models/llama/convert_llama_weights_to_hf.py --input_dir ./Llama-2-7b --model_size 7B --output_dir ./Llama-2-7b-hf
	
![转换过程](/img/2023/modelscope/hf-step.jpg "comand run")

**转换结果**：

![llama-7B-hf](/img/2023/modelscope/hf.jpg "llama-7B-hf")

# HF  to GGUF

GGUF格式，表示这个模型只支持CPU

	git clone https://github.com/ggerganov/llama.cpp.git
	pip install -r llama.cpp/requirements.txt
	python llama.cpp/convert.py -h

进行转换

	python llama.cpp/convert.py  Llama-2-7b-hf --outfile llama-2-7b-v1.5.gguf --outtype f32

![llama-7B-gguf](/img/2023/modelscope/gguf.jpg "llama-7B-gguf")

[How to convert HuggingFace model to GGUF format](https://github.com/ggerganov/llama.cpp/discussions/2948)


# HF to GPTQ

GPTQ格式，就是支持GPU。

	pip3 uninstall -y auto-gptq
	pip3 install gekko pandas
	git clone https://github.com/PanQiWei/AutoGPTQ
	cd AutoGPTQ
	pip3 install .

建立一个同级目录**Llama-2-7b-gptq**

	python3 quant_autogptq.py ./Llama-2-7b-hf ./llama-2-7b-gptq wikitext --bits 4 --group_size 128 --desc_act 0 --damp 0.1 --dtype float16 --seqlen 4096 --num_samples 128 --use_fast

use the **wikitext dataset** for quantisation，If your model is trained on something more specific, like code, or non-English language, then you may want to change to a different dataset. Doing that would require editing **quant_autogptq.py** to load an alternative dataset.


[How to convert HuggingFace model to GGPTQ format](https://huggingface.co/TheBloke/Llama-2-13B-chat-GPTQ/discussions/26)

# HF优势

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

如果你无法从官网下载Llama-2-7b的模型，可以参考。

	git clone https://www.modelscope.cn/angelala00/Llama-2-7b.git

