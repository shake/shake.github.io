---
layout:     post
title:      colab记事本
subtitle:   record all I use
date:       2024-01-02
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

已经成为Colab的深度用户，整理一下我colab使用的内容，基本都是和huggingface和大模型有关，其实也是方便自己查询。现在已经记不住东西。

# 密钥

登录huggingface是需要密钥，把colab的notebook分享出去，这个会导致你的密钥泄露。你是可以提前处理，但是总会有忘记的时候。

Colab提供密钥管理的功能，你可以把Huggingface的token存放并在colab进行调用。

非常完美，huggingface的登录，不需要进行交互，保护Huggingface的token，并且还给git提供相同的密钥。

	# import 密钥
	from google.colab import userdata
	hf_token = userdata.get('huggingface')
	!git config --global credential.helper store
	!huggingface-cli login --token $hf_token --add-to-git-credential
	
# 上传模型，数据集到huggingface

	!pip install -q huggingface_hub
	from huggingface_hub import create_repo, HfApi

	username = "chenshake"

	# Defined in the secrets tab in Google Colab
	api = HfApi(token=userdata.get("HF_TOKEN"))

	# Create empty repo
	create_repo(
		repo_id = f"{username}/{MODEL_NAME}-GGUF",
		repo_type="model",
		exist_ok=True,
	)

	# Upload gguf files
	api.upload_folder(
		folder_path=MODEL_NAME,
		repo_id=f"{username}/{MODEL_NAME}-GGUF",
		allow_patterns=f"*.gguf",
	)
	
	# upload folder
	api = HfApi()
	api.upload_folder(
		folder_path="Llama-2-7b-awq",
		repo_id="chenshake/Llama-2-7b-awq",
		repo_type="model",
	)

# 模型变量

简化工作量

	# models变量
	MODEL_ID = "meta-llama/Llama-2-7b"
	MODEL_NAME = MODEL_ID.split('/')[-1]


# 微调需要安装包

有时候需要很清楚自己使用的版本号，避免麻烦。transformers,colab是预安装，有时候会遇到需要最新版本。

	!pip install -q \
	git+https://github.com/huggingface/transformers.git \
	accelerate==0.21.0 \
	peft==0.4.0 bitsandbytes==0.40.2\ 
	trl==0.4.7

选择使用

	!pip install -q huggingface_hub
	!pip install -q -U trl transformers accelerate peft
	!pip install -q -U datasets bitsandbytes einops wandb

	# Uncomment to install new features that support latest models like Llama 2
	# !pip install git+https://github.com/huggingface/peft.git
	# !pip install git+https://github.com/huggingface/transformers.git
		
# GIT LFS

在[meta-llama/Llama-2-7b-hf](https://huggingface.co/meta-llama/Llama-2-7b-hf/tree/main)

里面有两种使用LFS存储大文件：PyTorch，safetensors。

PyTorch 模型权重使用Python的 pickle 工具将数据序列化到一个 .bin 文件中。 pickle 不安全，pickle 的文件可能包含可以执行的恶意代码。

safetensors 是 pickle 的一个安全替代方案，非常适合共享模型权重。safetensors 是一种安全快速存储和加载tensors的文件格式。

所以对你有用的是safetensors，无需下载bin格式

	git lfs clone "https://huggingface.co/meta-llama/Llama-2-7b-hf" --exclude="*.bin"
	
这样可以实现存放在LFS存储的文件，不会下载bin的文件，其他都会下载。

如果你不希望下载任何lfs的文件

	export GIT_LFS_SKIP_SMUDGE=1

# colab模型文件位置

	# make a soft link for  /root/.cache
	!mkdir -p /root/.cache/huggingface
	!ln -s /root/.cache/huggingface /content
	
# 文件下载

	!huggingface-cli download \
		--local-dir=/content/Llama-2-7b \
		meta-llama/Llama-2-7b \
		checklist.chk consolidated.00.pth params.json \
		tokenizer.model tokenizer_checklist.chk
		
		
# colab 环境检测

	# check default cuda version
	!nvcc --version 4

	# check default torch version
	import torch
	print(torch.__version__)

	# check default transformers version
	import transformers
	print (transformers.__version__)