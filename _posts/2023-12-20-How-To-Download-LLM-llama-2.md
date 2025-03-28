---
layout:     post
title:      How To Download LLM Llama 2
subtitle:   国内高效下载HuggingFace大模型
date:       2023-12-20
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

下载大模型，都是通过huggingface。以前Llama 2的下载，必须通过Meta才能下载，直接使用Meta的代码去Meta官方下载，国内是很容易中断，导致无法下载。现在你通过了Meta申请通过后，可以直接在huggingface进行下载。

![meta](/img/2023/colab/meta.jpg "meta")

# 介绍

国内从HuggingFace 下载大模型，也不容易，目前国内已经是无法直接访问HuggingFace。各种的mirror，代理，其实都很难保证你成功下载。

我需要把下载的大模型，放到阿里的ModelScope模型库，方便我使用ModelScope提供的虚拟机进行大模型微调。

没有太好的一步到位的办法，只能通过中转多次，才上传到ModelScope的模型库

HuggingFace --> Colab --> 阿里云盘 --> ModelScope虚拟机--> ModelScope模型库

本来最简单的方式，应该是Colab下载完成后，直接上传到ModelScope模型库，这个是没问题的。通过ModelScope API 是可以实现，就是速度太慢，缺少网盘秒传的功能，只能放弃。

采用这个方案的优势

* colab下载大模型速度快，大约5分钟。
* 阿里云盘上传快，因为网盘秒传特性，多大的模型基本都是3分钟。
* 网盘的CLI，可以快速下载到阿里的虚拟机上，大约5-10分钟。
* Git或Python SDK上传大模型到ModelScope模型库，速度保证，大约10分钟。

国外的网速和阿里云内部的网速是差不多的。一个100G 的大模型，无论下载和上传，都不是一件容易的事情，对基础设施，真的是一个很大的考验。尤其大模型的微调，就类似开发的CI过程，对git仓库是一个考验。

使用Llama-2-7b这个大模型做例子。colab代码，基本都是参考[huggingface-China](https://github.com/playmobil/huggingface-China)

感谢作者的分享，在调整过程中，确实是学到不少东西。

[colab](https://github.com/shake/LargeLanguageModelsProjects/blob/main/colab_download_lama2_13b_upload_to_alipan.ipynb)

这个colab，已经很完美。大家体验一下。我是提前把注释和代码填上，包括token，确认没有问题，再逐步运行，还可以选择：**runtime-->run all** 全部运行,比脚本运行还爽，提前把token填好，整个过程已经是无需交互。

# Colab

	# 安装需要的包
	!pip install gradio huggingface_hub aligo

	# import
	import os
	import shutil
	import huggingface_hub as hh
	import pandas as pd
	
	# 下载llama 2，需要使用HuggingFace的token通过验证才能下载，其他模型，这一步可以省掉。
	!huggingface-cli login --token hf_ziljuWSLzXXrlDvrUuB

	# 下载模型，设置huggingface的repo_id，更换不同的模型，
	# 只需要在repo_id设置就可以。其他地方无需调整。
	repo_id = "meta-llama/Llama-2-7b"
	repo_name = repo_id.replace("/","---")

	# 定义容量显示和下载路径

	def format_size(bytes, precision=2):
		"""
		Convert a file size in bytes to a human-readable format like KB, MB, GB, etc.
		Huggingface use 1000 not 1024
		"""
		units = ["B", "KB", "MB", "GB", "TB", "PB"]
		size = float(bytes)
		index = 0

		while size >= 1000 and index < len(units) - 1:
			index += 1
			size /= 1000

		return f"{size:.{precision}f} {units[index]}"


	def list_repo_files_info(repo_id,token=None):
		data_ls = []
		for file in list(hh.list_files_info(repo_id)):
			data_ls.append([file.path,format_size(file.size)])
		files = [file[0] for file in data_ls]
		data = pd.DataFrame(data_ls,columns = ['文件名','大小'])
		return data, files

	# 模型下载到当前目录下的"./download"目录
	def download_file(repo_id,filenames):
		print(filenames)
		repo_name = repo_id.replace("/","---")

		for filename in filenames:
			print(filename)
			out = hh.hf_hub_download(repo_id=repo_id,filename=filename,local_dir=f"./download/{repo_name}",local_dir_use_symlinks=False,force_download =True)
		out_path = f"./download/{repo_name}"
		return out_path




## 查看模型文件大小

	# 查看模型的文件
	data, filenames = list_repo_files_info(repo_id)
	filenames

## 下载模型

	# 开始下载模型
	out_path = download_file(repo_id,filenames)
	
## 查看模型下载结果

	# 查看模型的大小和文件

	import os.path
	from pathlib import Path
	path ="/content/download/"
	path=os.path.join(path, repo_name)
	print(path)
	
	!du -sh $path
	!ls -lsh $path

可以在Colab看到下载的大模型

![路径](/img/2023/colab/download.jpg "路径")

# 阿里云盘

这个地方，最麻烦的就是拿到云盘的refresh_token。

## 获取网盘refresh_token

打开Chrome，登录阿里网盘，chrome浏览器的右上角的3个点。或者用快捷方式：**ctrl+shift+i**

![dev-tool](/img/2023/colab/dev-tool.jpg "dev-tool")

找到**应用** 标签：

![refresh_token](/img/2023/colab/refresh.jpg "refresh_token")

## 环境配置

下面的操作，其实都是在Colab上进行。

	# 上传阿里云盘，填上token
	from aligo import Aligo
	refresh_token = "自己的token"
	ali = Aligo(refresh_token=refresh_token)

	# 获取用户信息和获取网盘根目录文件列表
	user = ali.get_user()
	print(user.user_name, user.nick_name, user.phone)
	ll = ali.get_file_list() 
	ll


## 上传模型到阿里网盘

	# 无法指定文件夹上传，只能传到根目录下，估计是cli的bug
	remote_folder = ali.get_folder_by_path(out_path, create_folder=True)
	ali.upload_folder(out_path)
	
![yunpan](/img/2023/colab/yunpan.jpg "yunpan")

# ModelScope虚拟机

我的所有操作，都记录在notebook。包括模型的下载和上传到ModelScope模型库

[notebook](https://github.com/shake/LargeLanguageModelsProjects/blob/main/alipan_download_and_upload_ModelScope.ipynb)

登录ModelScope社区，启动虚拟机。

![md](/img/2023/colab/md-1.jpg "md")

登录虚拟机，可以用终端，也是可以通过notebook，就是比较难用，这次我还尽量在notebook来完成操作。

[阿里云盘操作手册](https://github.com/tickstep/aliyunpan/blob/main/docs/manual.md)

通过阿里云盘CLI，实现大模型下载。

	# 云盘linux客户端
	!curl -fsSL http://file.tickstep.com/apt/pgp | gpg --dearmor | tee /etc/apt/trusted.gpg.d/tickstep-packages-archive-keyring.gpg > /dev/null && echo "deb [signed-by=/etc/apt/trusted.gpg.d/tickstep-packages-archive-keyring.gpg arch=amd64,arm64] http://file.tickstep.com/apt aliyunpan main" | tee /etc/apt/sources.list.d/tickstep-aliyunpan.list > /dev/null && apt-get update && apt-get install -y aliyunpan

	# CLI通过RefreshToken登录
	!aliyunpan login -RefreshToken=<网盘的RefreshToken，上传网盘使用的那个RefreshToken>

	# 设置下载存储路径
	!aliyunpan config set -savedir /mnt

	# 查看云盘根目录
	!aliyunpan ls

	# 下载云盘大模型
	!aliyunpan download meta-llama---Llama-2-7b
	
	# 查看下载结果
	!du -sh /mnt
	!ls -lsh /mnt
	
云盘CLI,会对目录生成一串数字，目录下，才是模型

	# 一串数字改成huggingface
	!mv 127d6e7249c548cbb773138c67178ea2 huggingface

ModelScope要求模型上传必须包含**configuration.json** 文件，所以专门在repo里创建一个空文件。

	# 创建一个空文件，满足repo上传ModelScope要求
	!touch ./huggingface/meta-llama---Llama-2-7b/configuration.json


# ModelScope模型库

把模型上传到模型库，可以用git或SDK。

## 创建新模型

首先需要在ModelScope模型库，创建一个新的模型

![md7](/img/2023/colab/md7.jpg "md7")

顺便把从huggingface相应的模型下载REDME，直接上传就可以，这样就有模型的介绍。

进入创建的模型

![md3](/img/2023/colab/md3.jpg "md3")

目前模型文件只有2个文件
![md4](/img/2023/colab/md4.jpg "md4")



## 模型上传

**参考文档**

* [ModelScope 模型下载](https://modelscope.cn/docs/%E6%A8%A1%E5%9E%8B%E7%9A%84%E4%B8%8B%E8%BD%BD)
* [ModelScope模型上传](https://modelscope.cn/docs/%E6%A8%A1%E5%9E%8B%E7%9A%84%E4%B8%8A%E4%BC%A0)



### Python SDK上传模型

获取令牌

![md5](/img/2023/colab/md5.jpg "md5")

上传

	# 记得替换ModelScope token，
	# repo必须包含一个configuration.json文件
	from modelscope.hub.api import HubApi
	YOUR_ACCESS_TOKEN = 'f368c21f-2ade-4ac0-876f-8bfb5d'
	api = HubApi()
	api.login(YOUR_ACCESS_TOKEN)
	api.push_model(
		model_id="shakechen/Llama-2-7b", 
		model_dir="/mnt/huggingface/meta-llama---Llama-2-7b" 
	)

大概10分钟，就上传完毕。可以在模型文件下看到上传的文件。

对比HuggingFace的repo的文件，会发现文件的大小有差异，这个是因为HuggingFace按G是1000M，不是1024M导致的。整个过程，就多了一个**configuration.json** 空文件。

![md6](/img/2023/colab/md6.jpg "md6")