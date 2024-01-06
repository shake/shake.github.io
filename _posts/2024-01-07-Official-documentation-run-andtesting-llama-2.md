---
layout:     post
title:      Official code run and testing Llama-2 
subtitle:   官方文档运行和测试Llama-2 
date:       2023-10-11
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

Llama-2的原始模型，其实我没运行过，尝试照着官方的文档运行，16G的显存，运行失败，看来是必须使用24G显存，才能直接运行起来。

这次是在阿里的魔搭进行实验。

	%cd /mnt/
	!ls

# Llama-2-7b

**下载Llama-2-7b**

	!git clone https://www.modelscope.cn/shakechen/Llama-2-7b.git

**下载Facebook 代码**

	!git clone https://github.com/facebookresearch/llama.git

**安装**

	%cd llama
	!pip install -e .

**切换目录**

	%cd /mnt/


**运行Llama-2-7b**

	!torchrun --nproc_per_node 1 /mnt/llama/example_text_completion.py \
		 --ckpt_dir /mnt/Llama-2-7b \
		 --tokenizer_path /mnt/Llama-2-7b/tokenizer.model \
		 --max_seq_len 128 --max_batch_size 4
	 
**输出结果**

	Loaded in 36.93 seconds
	I believe the meaning of life is
	> to be happy. I believe we are all born with the potential to be happy. The meaning of life is to be happy, but the way to get there is not always easy.
	The meaning of life is to be happy. It is not always easy to be happy, but it is possible. I believe that

	==================================

	Simply put, the theory of relativity states that 
	> 1) time, space, and mass are relative, and 2) the speed of light is constant, regardless of the relative motion of the observer.
	Let’s look at the first point first.
	Relative Time and Space
	The theory of relativity is built on the idea that time and space are relative

	==================================

	A brief message congratulating the team on the launch:

			Hi everyone,
			
			I just 
	> wanted to say a big congratulations to the team on the launch of the new website.

			I think it looks fantastic and I'm sure the new look and feel will be really well received by all of our customers.

			I'm looking forward to the next few weeks as

	==================================

	Translate English to French:
			
			sea otter => loutre de mer
			peppermint => menthe poivrée
			plush girafe => girafe peluche
			cheese =>
	> fromage
			fish => poisson
			giraffe => girafe
			elephant => éléphant
			cat => chat
			giraffe => girafe
			elephant => éléphant
			cat => chat
			giraffe => gira

	==================================

**使用代码提供的例子**

	!torchrun --nproc_per_node 1 /mnt/llama/example_text_completion.py \
		--ckpt_dir /mnt/Llama-2-7b/ \
		--tokenizer_path /mnt/Llama-2-7b/tokenizer.model \
		--max_seq_len 128 --max_batch_size 4


**输出结果**


	Loaded in 50.77 seconds
	I believe the meaning of life is
	> to be happy. I believe we are all born with the potential to be happy. The meaning of life is to be happy, but the way to get there is not always easy.
	The meaning of life is to be happy. It is not always easy to be happy, but it is possible. I believe that

	==================================

	Simply put, the theory of relativity states that 
	> 1) time, space, and mass are relative, and 2) the speed of light is constant, regardless of the relative motion of the observer.
	Let’s look at the first point first.
	Relative Time and Space
	The theory of relativity is built on the idea that time and space are relative

	==================================

	A brief message congratulating the team on the launch:

			Hi everyone,
			
			I just 
	> wanted to say a big congratulations to the team on the launch of the new website.

			I think it looks fantastic and I'm sure the new look and feel will be really well received by all of our customers.

			I'm looking forward to the next few weeks as

	==================================

	Translate English to French:
			
			sea otter => loutre de mer
			peppermint => menthe poivrée
			plush girafe => girafe peluche
			cheese =>
	> fromage
			fish => poisson
			giraffe => girafe
			elephant => éléphant
			cat => chat
			giraffe => girafe
			elephant => éléphant
			cat => chat
			giraffe => gira

	==================================


# chat版本

**下载chat版本**

	!git clone https://www.modelscope.cn/shakechen/Llama-2-7b-chat.git

**运行**

	!torchrun --nproc_per_node 1 /mnt/llama/example_chat_completion.py \
		--ckpt_dir /mnt/Llama-2-7b-chat/ \
		--tokenizer_path /mnt/Llama-2-7b-chat/tokenizer.model \
		--max_seq_len 128 --max_batch_size 4


运行失败，感觉还是显存不足导致，24G的显存，还是无法运行chat的版本。