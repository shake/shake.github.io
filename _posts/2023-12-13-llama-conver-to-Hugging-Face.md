---
layout:     post
title:      LLama2-7B Models Quantization Method 
subtitle:   Quantization Method GGUF GPTQ 
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

如果你无法从官网下载Llama-2-7b的模型，可以参考,git clone ，删除多余的文件,我的实验也是使用这个repo来完成。

	git clone https://www.modelscope.cn/angelala00/Llama-2-7b.git

下载完成后，需要删除没必要的文件

	cd Llama-2-7b/
	rm -rf .git .gitattributes configuration.json gitattributes LICENSE.txt README.md USE_POLICY.md Responsible-Use-Guide.pdf 
	mkdir 7B
	cd ..


下载回来的repo是26G，删除隐藏文件后，变成13G。

## conda

	conda create -n shake python=3.11
	conda activate shake

## 安装所需库

   首先确保你已经安装了`transformers`和`torch`库。需要确认是否使用GPU，如果是GPU，需要确认显卡支持的Cuda的版本。

### GPU

	nvidia-smi

Cuda的版本为11.8.

![cuda](/img/2023/modelscope/cuda.jpg "cuda")


	pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
	pip3 show torch
	pip3 install transformers

### CPU

	pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
	pip3 install transformers


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

	python llama.cpp/convert.py  Llama-2-7b-hf --outfile llama-2-7b-Q8_0.gguf --outtype q8_0
	
outtype的选项有： 'f32', 'f16', 'q8_0'


	(shake) root@eais-bjrs3yp8aak1upawsmjg-54f5dcbfbc-tvpsm:/mnt# ls llama-2-7b-Q8_0.gguf -lash
	6.7G -rw-r--r-- 1 root root 6.7G 12月 14 19:01 llama-2-7b-Q8_0.gguf



[How to convert HuggingFace model to GGUF format](https://github.com/ggerganov/llama.cpp/discussions/2948)


# HF to GPTQ（有问题）

GPTQ格式，就是支持GPU。必须有GPU的虚拟机，装上Cuda，才能进行转换。

	pip3 uninstall -y auto-gptq
	pip3 install gekko pandas
	git clone https://github.com/PanQiWei/AutoGPTQ
	cd AutoGPTQ
	pip3 install .

在Llama-2-7b ,Llama-2-7b-hf,同级的目录下,建立目录**Llama-2-7b-gptq**

创建一个文件**quant_autogptq.py**

[quant_autogptq.py](https://gist.github.com/TheBloke/b47c50a70dd4fe653f64a12928286682#file-quant_autogptq-py)

最终你看到的目录

	(shake) root@eais-bjrs3yp8aak1upawsmjg-54f5dcbfbc-tvpsm:/mnt# ls -lash
	总计 6.7G
	4.0K drwxr-xr-x  1 root root 4.0K 12月 14 19:20 .
	4.0K drwxr-xr-x  1 root root 4.0K 12月 14 18:43 ..
	4.0K drwxr-xr-x 11 root root 4.0K 12月 14 19:14 AutoGPTQ
	4.0K drwxr-xr-x  3 root root 4.0K 12月 14 18:48 Llama-2-7b
	4.0K drwxr-xr-x  2 root root 4.0K 12月 14 19:14 Llama-2-7b-gptq
	4.0K drwxr-xr-x  2 root root 4.0K 12月 14 18:57 Llama-2-7b-hf
	6.7G -rw-r--r--  1 root root 6.7G 12月 14 19:01 llama-2-7b-Q8_0.gguf
	4.0K drwxr-xr-x 19 root root 4.0K 12月 14 18:44 llama.cpp
	 12K -rw-r--r--  1 root root  11K 12月 14 19:20 quant_autogptq.py
	4.0K drwxr-xr-x 15 root root 4.0K 12月 14 18:44 transformers

进行转换

	python3 quant_autogptq.py ./Llama-2-7b-hf ./llama-2-7b-gptq wikitext --bits 4 --group_size 128 --desc_act 0 --damp 0.1 --dtype float16 --seqlen 4096 --num_samples 128 --use_fast

use the **wikitext dataset** for quantisation，If your model is trained on something more specific, like code, or non-English language, then you may want to change to a different dataset. Doing that would require editing **quant_autogptq.py** to load an alternative dataset.

出现错误，应该是网络的原因，这个暂时无法解决。

	(shake) root@eais-bjrs3yp8aak1upawsmjg-54f5dcbfbc-tvpsm:/mnt# python3 quant_autogptq.py ./Llama-2-7b-hf ./llama-2-7b-gptq wikitext --bits 4 --group_size 128 --desc_act 0 --damp 0.1 --dtype float16 --seqlen 4096 --num_samples 128 --use_fast
	2023-12-14 19:27:17 INFO [__main__] Loading tokenizer
	^FTraceback (most recent call last):
	  File "/mnt/quant_autogptq.py", line 231, in <module>
		quantizer.run_quantization()
	  File "/mnt/quant_autogptq.py", line 134, in run_quantization
		traindataset = self.get_wikitext2()
					   ^^^^^^^^^^^^^^^^^^^^
	  File "/mnt/quant_autogptq.py", line 50, in get_wikitext2
		wikidata = load_dataset('wikitext', 'wikitext-2-raw-v1', split='test')
				   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
	  File "/opt/conda/envs/shake/lib/python3.11/site-packages/datasets/load.py", line 2128, in load_dataset
		builder_instance = load_dataset_builder(
						   ^^^^^^^^^^^^^^^^^^^^^
	  File "/opt/conda/envs/shake/lib/python3.11/site-packages/datasets/load.py", line 1814, in load_dataset_builder
		dataset_module = dataset_module_factory(
						 ^^^^^^^^^^^^^^^^^^^^^^^
	  File "/opt/conda/envs/shake/lib/python3.11/site-packages/datasets/load.py", line 1511, in dataset_module_factory
		raise e1 from None
	  File "/opt/conda/envs/shake/lib/python3.11/site-packages/datasets/load.py", line 1467, in dataset_module_factory
		raise ConnectionError(f"Couldn't reach '{path}' on the Hub ({type(e).__name__})")
	ConnectionError: Couldn't reach 'wikitext' on the Hub (ConnectionError)
	(shake) root@eais-bjrs3yp8aak1upawsmjg-54f5dcbfbc-tvpsm:/mnt# 


[How to convert HuggingFace model to GGPTQ format](https://huggingface.co/TheBloke/Llama-2-13B-chat-GPTQ/discussions/26)

