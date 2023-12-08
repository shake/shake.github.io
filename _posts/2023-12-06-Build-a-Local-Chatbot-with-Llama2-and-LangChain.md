---
layout:     post
title:      llama2 and Langchain 搭建聊天机器人（1）
subtitle:   Build a Local Chatbot with Llama2 and LangChain
date:       2023-12-6
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

开始写文章的时候，其实我还没开始安装，先把整个过程记录下来。

阿里大模型社区魔搭，提供36小时的一台GPU虚拟机，我想我应该可以充分利用这个资源，来完成机器人的搭建过程。

感觉这是一个庞大工程，对我来说，需要分几篇文章来熟悉和了解。


# 模型分类

通过 [HuggingFace](https://huggingface.co/) 注册和下载相应的大模型。一个模型，给你提供的选项也很多，搞清楚他们的区别，其实也是非常不容易。就针对Llama 2来说。

对于大模型来说，参数是一个重要的指标，7B，13B，70B。参数越多，肯定就是所谓越好，但是运行起来需要的资源就更多。

现在模型根据使用的场景和任务，也分成chat和非chat任务的模型。

研发需要下载大模型的代码来进行训练，对于用户来说，需要下载已经训练好的大模型来本地玩。

训练好的大模型，一般来说，大模型运行，需要GPU的支持，来进行推理，但是很多时候，没有GPU，也是可以使用CPU来进行推理，所以大模型一般都会提供GPU版本和CPU版本。

模型大小和精度，精度越高，训练成本越高，模型越大。

# 选择模型

	TheBloke/Llama-2-13B-chat-GGUF
	TheBloke/Llama-2-13B-chat-GGML
	TheBloke/Llama-2-13B-chat-GPTQ

GGML是已经淘汰的格式，表示支持CPU，GGUF是新的支持CPU的格式。GPTQ是支持GPU的格式。

# Llama.cpp介绍

Llama.cpp 是一个开源库，用于在 C++ 中使用大型语言模型（LLM）。它基于 Facebook 的 GGUF 架构，支持各种 LLM，包括 Llama、Falcon、Alpaca 等。

Llama.cpp 提供了以下功能：

* 加载和使用 LLM
* 生成文本、翻译语言、编写不同类型的创意内容等
* 使用 LLM 进行推理

简单理解，就是把Llama.cpp安装好后，直接把大模型放到目录下，就可以运行。

# 魔搭安装Llama 2

魔搭提供大模型的下载，还有内部的pip源，这样可以大幅加快大模型的部署。

终端登录到魔搭提供的免费虚拟机，8core，32G内存。

* 安装llama.cpp,需要使用make编译。
* 下载相应的大模型，通过魔搭下载，速度很快。
* 启动大模型，可以支持浏览器访问。

![魔搭](/img/2023/modelscope/notebook.jpg "notebook")
![魔搭](/img/2023/modelscope/notebook1.jpg "notebook")


整个过程，其实很简单，通过一个脚本运行，就可以搞定。

脚本出处：[github](https://github.com/sychhq/llama-cpp-setup)
脚本有一个bug，目前llama.cpp不支持GGML，只支持GGNF。[问题讨论](https://github.com/sychhq/llama-cpp-setup/issues/4)

镜像里git，make，curl，已经是提前预装。

	cd /root/
	git clone https://github.com/shake/llama-cpp-setup.git && cd llama-cpp-setup && chmod +x setup.sh && ./setup.sh`

![llama 2](/img/2023/modelscope/finish.jpg "llama")

![ask question](/img/2023/modelscope/ai.jpg "question")










# 参考

* [llama2+langchain构建本地化的中文QA系统
](https://zhuanlan.zhihu.com/p/652172969)

* [colab](https://colab.research.google.com/drive/1Ssg-fffeJ0LG0m3DoTofeLPvOUQyG1h3?usp=sharing#scrollTo=SP4Bk5YBf1mI)
* [notebook](https://github.com/madfrog/chatbot_llama2/blob/main/chatbot_demo.ipynb)

* [LangChain QuickStart with Llama 2
](
https://www.mlexpert.io/prompt-engineering/langchain-quickstart-with-llama-2)


