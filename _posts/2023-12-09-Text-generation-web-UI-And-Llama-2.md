---
layout:     post
title:      Text generation web UI And Llama 2
subtitle:   Manual Installation of Text generation web UI
date:       2023-12-09
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

为了加深自己对AI涉及的软件和工具深入了解。这次采用手工的方式进行安装Text generation web UI And Llama 2.

感谢 [Text generation web UI](https://github.com/oobabooga/text-generation-webui)
安装文档写的如此详细，我只是重复做了一遍，记录下来。

# Install Conda

Conda 是一个开源包管理系统和环境管理系统，用于安装、运行和更新软件包及其依赖项。它适用于 Windows、macOS 和 Linux 平台，是为 Python 程序创建的，但可以打包和分发适用于任何语言的软件。

Conda 的主要用途包括：

* **安装软件包和依赖项**：Conda 可以从 Anaconda 存储库或其他第三方存储库中安装软件包。它还可以自动安装软件包的依赖项，从而简化软件包管理。
* **创建和管理虚拟环境**：Conda 可以创建和管理虚拟环境。虚拟环境是独立的软件包环境，可以用于隔离不同项目的软件包依赖项。
* **打包和分发软件**：Conda 可以用于打包和分发软件。它可以创建包含软件包、依赖项和配置文件的软件包。

[官方说明](https://educe-ubc.github.io/conda.html)

	curl -sL "https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh" > "Miniconda3.sh"
	bash Miniconda3.sh

# Create a new conda environment

	conda create -n shake python=3.11
	conda activate shake
	
# Install Pytorch


PyTorch 是一个用于构建深度学习模型的开源框架。它使用 Python 编写，因此对于大多数机器学习开发者而言，学习和使用起来相对简单。PyTorch 的独特之处在于，它完全支持 GPU，并且使用反向模式自动微分技术，因此可以动态修改计算图形。

## CPU

Pytorch安装选项有点多，需要根据实际情况来选择。

[官网](https://pytorch.org/get-started/locally/)


	pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu

## GPU

	nvidia-smi
	pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

# Install the web UI


需要检测cpu是否支持avx2。

	lscpu | grep avx2
	
## CPU
	
也是需要根据情况来选择不同的依赖。

	git clone https://github.com/oobabooga/text-generation-webui
	cd text-generation-webui
	pip install -r requirements_cpu_only.txt
	
## GPU

	git clone https://github.com/oobabooga/text-generation-webui
	cd text-generation-webui
	pip install -r requirements.txt


# 下载大模型

text-generation-webui目录下，魔搭手工下载大模型 llama-2-13b-chat.Q4_K_M.gguf

## CPU

curl -LO "https://modelscope.cn/api/v1/models/Xorbits/Llama-2-13b-Chat-GGUF/repo?Revision=master&FilePath=llama-2-13b-chat.Q4_K_M.gguf"
mv 'repo?Revision=master&FilePath=llama-2-13b-chat.Q4_K_M.gguf' models/llama-2-13b-chat.Q4_K_M.gguf


最近curl -o指定输出位置，会导致到出现错误，**curl: (6) Could not resolve host**，没法解决，所以你可以像上面的先通过curl下载回来，再移动和修改名字，或者改成用git的来下载大模型。

	git clone https://www.modelscope.cn/Xorbits/Llama-2-13b-Chat-GGUF.git



下载完成后，在移动到**models** 目录下。git需要支持LFS，魔搭默认已经支持，如果不支持，可以参考下面命令。

	curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | sudo bash
	sudo apt-get install git-lfs
	git lfs install

## GPU

	git clone https://www.modelscope.cn/Cookize/Llama-2-13B-chat.git

大模型都是放在**models**下面

	text-generation-webui
	├── models
	│   ├── llama-2-13b-chat.Q4_K_M.gguf


# 启动web

	python server.py
	
![web 访问地址](/img/2023/modelscope/link.jpg "外网地址")

魔搭做了网关的映射，直接点击，就可以web访问。

# Text generation web UI

Text generation web UI配置还是比较复杂，可以设置的地方太多。而且迭代更新很快。慢慢琢磨。


