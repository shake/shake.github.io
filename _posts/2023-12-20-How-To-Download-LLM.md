---
layout:     post
title:      How To Download LLM Llama 2
subtitle:   国内高效下载大模型
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

HuggingFace --> Colab --> 阿里云盘 --> 魔搭虚拟机--> ModelScope模型库

本来最简单的方式，应该是Colab下载完成后，直接上传到ModelScope模型库，这个是没问题的。通过ModelScope API 是可以实现，就是速度太慢，缺少网盘秒传的功能，只能放弃。

采用这个方案的优势

* colab下载大模型速度快，大约5分钟。
* 阿里云盘上传快，因为网盘秒传特性，多大的模型基本都是3分钟。
* 网盘的CLI，可以快速下载到阿里的虚拟机上，大约5-10分钟。
* 内网Git上传大模型到ModelScope模型库，速度保证，大约5-10分钟。

国外的网速和阿里云内部的网速是差不多的。一个100G 的大模型，无论下载和上传，都不是一件容易的事情，对基础设施，真的是一个很大的考验。尤其大模型的微调，就类似开发的CI过程，对git仓库是一个考验。

使用Llama-2-7b这个大模型做例子。colab代码，基本都是参考[huggingface-China](https://github.com/playmobil/huggingface-China)

感谢作者的分享，在调整过程中，确实是学到不少东西。

[我的第一个Colab](https://github.com/shake/LargeLanguageModelsProjects/blob/main/Colab_download_LLM_and_upload_to_aliyunpan.ipynb)

# Colab

	# 安装需要的包
	!pip install gradio huggingface_hub aligo

	# import
	import os
	import shutil
	import huggingface_hub as hh
	import pandas as pd
	
	# 下载llama 2，需要使用HuggingFace的token通过验证才能下载，其他模型，这一步可以省掉。
	#有一个方框，输入token。
	!huggingface-cli login

	# 下载模型，设置huggingface的repo_id，更换不同的模型，
	# 只需要在这个地方设置就可以。后面内容不需要调整。
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

![HuggingFace](/img/2023/colab/hf-cli.jpg "HuggingFace")


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
	
	查看模型大小
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

	# 填上token
	from aligo import Aligo
	refresh_token = "自己的token"
	ali = Aligo(refresh_token=refresh_token)

	# 获取用户信息和获取网盘根目录文件列表
	user = ali.get_user()
	print(user.user_name, user.nick_name, user.phone)
	ll = ali.get_file_list() 
	ll


## 上传大模型到阿里网盘

	# 无法指定文件夹上传，只能传到根目录下，估计是cli的bug
	remote_folder = ali.get_folder_by_path()
	ali.upload_folder(out_path)
	
![yunpan](/img/2023/colab/yunpan.jpg "yunpan")