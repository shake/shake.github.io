---
layout:     post
title:      Mac Min 4 Install ConfyUI Flux
subtitle:   Mac Min 4 安装ConfyUI Flux
date:       2024-11-22
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

今年给自己准备了一个生日礼物，就是mac-mini 4，16G内存，256G存储空间。 这也算是我个人的第一台mac设备。打算如何折腾呢。先确认一下，运行Flux的效果如何。

## 准备

### git

在mac安装git，需要先安装homebrew [官网地址](https://git-scm.com/downloads/mac)


安装homebrew

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

```


安装git

```
brew install git

```


## confyui

现在开源软件安装，官网都写的很详细，照做就基本可以。 [Mac安装链接](https://github.com/comfyanonymous/ComfyUI?tab=readme-ov-file#installing)




### conda

```
curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh
sh Miniconda3-latest-MacOSX-arm64.sh
```


创建一个confyUI的环境

```
conda create -n confyui python=3.12.4
```


进入conda

```
coda activate confyui
```



### PyTorch

```
pip3 install --pre torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/nightly/cpu
```


验证一下安装是否正常

```
import torch
if torch.backends.mps.is_available():
    mps_device = torch.device("mps")
    x = torch.ones(1, device=mps_device)
    print (x)
else:
    print ("MPS device not found.")

```


正常会看到

```
tensor([1.], device='mps:0')
```


退出

```
quit()
```




### confyui

安装confyui，非常简单，只需要git下载就可以。目前confyui更新非常快，只能下载master就可以。


```
git clone https://github.com/comfyanonymous/ComfyUI.git
cd confyui
pip install -r requirement.txt
```


安装完成后，可以直接运行

```
python main.py
```



## Flux

Flux安装，其实就是把相应的文件，放到指定目录下就可以。

* 下载 [Flux dev -fp8](https://huggingface.co/Comfy-Org/flux1-dev/blob/main/flux1-dev-fp8.safetensors) 放到 <span style="color: black; font-weight: bold;">ComfyUI/models/diffusion_models/  </span>文件夹下;
* 下载 [clip_l.safetensors](https://huggingface.co/comfyanonymous/flux_text_encoders/tree/main) 和 [t5xxl_fp8_e4m3fn.safetensors](https://huggingface.co/comfyanonymous/flux_text_encoders/tree/main),放到 <span style="color: black; font-weight: bold;">ComfyUI/models/clip/ </span> 文件下；
* 下载 [VAE](https://huggingface.co/black-forest-labs/FLUX.1-schnell/blob/main/ae.safetensors) 放到<span style="color: black; font-weight: bold;">ComfyUI/models/vae/ </span> 文件夹下


这时候打开 ComfyUI 本地访问地址

把下面照片，保存到本地，上传到ComfyUI上，就可以看到第一条工作流。

![官方图片](/img/2024/art/flux_dev_checkpoint_example.png "picture")


尝试下面提示词

```
8k hi res landscape photograph of a Bavarian castle taken from
a drone
```


