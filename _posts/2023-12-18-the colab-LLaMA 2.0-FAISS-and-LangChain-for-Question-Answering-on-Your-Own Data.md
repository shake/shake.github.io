---
layout:     post
title:      LLaMA2 FAISS LangChain for Question-Answering
subtitle:   Colab Deploy LLaMA 2.0, FAISS and LangChain
date:       2023-12-18
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

# Colab

Google 提供的colab实在是太方便了。免费用户，可以使用T4的GPU，通过这个基本可以完成很多实验。这次 LLaMA 2.0（大模型）, FAISS（向量数据库） and LangChain（链） 来实现一个最问答系统的demo。这次实验是通过notebook来完成，把整个操作的过程都记录下来。

[Using LLaMA 2.0, FAISS and LangChain for Question-Answering on Your Own Data](https://medium.com/@murtuza753/using-llama-2-0-faiss-and-langchain-for-question-answering-on-your-own-data-682241488476)


# HuggingFace介绍

HuggingFace，可以理解成AI圈的github，在开发模型上，HuggingFace做的很多基础性的工作。

Llama-2这个是目前开源领域资料最丰富的大模型，用来学习，是非常方便的。

![LLAMA](/img/2023/colab/llama.jpg "llama")

大家在Huggingface上看到的版本，Llama2和Llama2-chat的6个版本，是Meta发布的，你需要使用相同的邮箱申请Meta和Huggingface的账号，通过审批后，可以在Huggingface访问和下载。

Llama2-hf和Llama2-hf，是Huggingface和Meta合作，加上Huggingface的Transformers，发布了Llama2-hf和Llama2-hf版本，适合开发者对模型进行微调。

使用工具，就可以实现从Llama的原始模型，转换成HF模型，目前Llama2的hf版本，也是由Meta发布的。

TheBloke，一个大模型的开发者（真名是：Tom Jobbins），他对各个厂商的大模型就是微调，把不同参数的微调的大模型发布。

在Huggingface上，经常看到

![LLAMA](/img/2023/colab/all-type.jpg "all")

GGML,GGUF,GPTQ,AWQ，都是针对大模型进行压缩和优化，让他使用更少的资源，CPU和内存进行运行。

每种的压缩，后来都是不同的算法，算法优秀，压缩比更高，同时损失最小。目前GGML的压缩格式，已经淘汰，由GGUF来替代。

原始的模型，很多情况厂商并不会提供GGUF,GPTQ,AWQ格式。TheBloke对厂商的大模型进行了这种压缩转换，让用户更加方便来测试大模型。


# 配置环境

## 查看显卡

	!nvidia-smi

## 大模型微调所需要的包

	!pip install -q accelerate==0.21.0 peft==0.4.0 bitsandbytes==0.40.2 transformers==4.31.0 trl==0.4.7 faiss-gpu einops langchain xformers sentence_transformers torch==2.1.0

由于大模型的迅速发展，各个包的依赖，版本都会发生变化，如果出错，就根据错误提示，调整一下就可以。
	

# Hugging Face Pipeline



	from torch import cuda, bfloat16
	import transformers

	model_id = 'meta-llama/Llama-2-7b-chat-hf'

	device = f'cuda:{cuda.current_device()}' if cuda.is_available() else 'cpu'

	# set quantization configuration to load large model with less GPU memory
	# this requires the `bitsandbytes` library
	bnb_config = transformers.BitsAndBytesConfig(
		load_in_4bit=True,
		bnb_4bit_quant_type='nf4',
		bnb_4bit_use_double_quant=True,
		bnb_4bit_compute_dtype=bfloat16
	)

	# begin initializing HF items, you need an access token
	# hf_auth = <font color='red'>'hf_SFVATnoGtfsnyyZVwbSLyKTXR'</font>
	hf_auth = '<add your access token here>'
	model_config = transformers.AutoConfig.from_pretrained(
		model_id,
		use_auth_token=hf_auth
	)

	model = transformers.AutoModelForCausalLM.from_pretrained(
		model_id,
		trust_remote_code=True,
		config=model_config,
		quantization_config=bnb_config,
		device_map='auto',
		use_auth_token=hf_auth
	)

	# enable evaluation mode to allow model inference
	model.eval()

	print(f"Model loaded on {device}")


只需要把huggingface上你自己的token填上就可以。meta-llama/Llama-2-7b-chat-hf就会下载。


## tokenizer

The pipeline requires a tokenizer which handles the translation of human readable plaintext to LLM readable token IDs. The Llama 2 7B models were trained using the Llama 2 7B tokenizer, which can be initialized with this code:




	tokenizer = transformers.AutoTokenizer.from_pretrained(
		model_id,
		use_auth_token=hf_auth
	)
	

## 模型停止输出
	
	stop_list = ['\nHuman:', '\n```\n']

	stop_token_ids = [tokenizer(x)['input_ids'] for x in stop_list]
	stop_token_ids

## convert stop token ids into LongTensor objects.
	
	import torch

	stop_token_ids = [torch.LongTensor(x).to(device) for x in stop_token_ids]
	stop_token_ids


## 检查 token IDs

	from transformers import StoppingCriteria, StoppingCriteriaList

	# define custom stopping criteria object
	class StopOnTokens(StoppingCriteria):
		def __call__(self, input_ids: torch.LongTensor, scores: torch.FloatTensor, **kwargs) -> bool:
			for stop_ids in stop_token_ids:
				if torch.eq(input_ids[0][-len(stop_ids):], stop_ids).all():
					return True
			return False

	stopping_criteria = StoppingCriteriaList([StopOnTokens()])


## 定义参数


	generate_text = transformers.pipeline(
		model=model, 
		tokenizer=tokenizer,
		return_full_text=True,  # langchain expects the full text
		task='text-generation',
		# we pass model parameters here too
		stopping_criteria=stopping_criteria,  # without this model rambles during chat
		temperature=0.1,  # 'randomness' of outputs, 0.0 is the min and 1.0 the max
		max_new_tokens=512,  # max number of tokens to generate in the output
		repetition_penalty=1.1  # without this output begins repeating
	)

## 测试

	res = generate_text("Explain me the difference between Data Lakehouse and Data Warehouse.")
	print(res[0]["generated_text"])


# HF Pipeline in LangChain



	from langchain.llms import HuggingFacePipeline

	llm = HuggingFacePipeline(pipeline=generate_text)

	# checking again that everything is working fine
	llm(prompt="Explain me the difference between Data Lakehouse and Data Warehouse.")


## Ingesting Data using Document Loader

	from langchain.document_loaders import WebBaseLoader

	web_links = ["https://www.databricks.com/","https://help.databricks.com","https://databricks.com/try-databricks","https://help.databricks.com/s/","https://docs.databricks.com","https://kb.databricks.com/","http://docs.databricks.com/getting-started/index.html","http://docs.databricks.com/introduction/index.html","http://docs.databricks.com/getting-started/tutorials/index.html","http://docs.databricks.com/release-notes/index.html","http://docs.databricks.com/ingestion/index.html","http://docs.databricks.com/exploratory-data-analysis/index.html","http://docs.databricks.com/data-preparation/index.html","http://docs.databricks.com/data-sharing/index.html","http://docs.databricks.com/marketplace/index.html","http://docs.databricks.com/workspace-index.html","http://docs.databricks.com/machine-learning/index.html","http://docs.databricks.com/sql/index.html","http://docs.databricks.com/delta/index.html","http://docs.databricks.com/dev-tools/index.html","http://docs.databricks.com/integrations/index.html","http://docs.databricks.com/administration-guide/index.html","http://docs.databricks.com/security/index.html","http://docs.databricks.com/data-governance/index.html","http://docs.databricks.com/lakehouse-architecture/index.html","http://docs.databricks.com/reference/api.html","http://docs.databricks.com/resources/index.html","http://docs.databricks.com/whats-coming.html","http://docs.databricks.com/archive/index.html","http://docs.databricks.com/lakehouse/index.html","http://docs.databricks.com/getting-started/quick-start.html","http://docs.databricks.com/getting-started/etl-quick-start.html","http://docs.databricks.com/getting-started/lakehouse-e2e.html","http://docs.databricks.com/getting-started/free-training.html","http://docs.databricks.com/sql/language-manual/index.html","http://docs.databricks.com/error-messages/index.html","http://www.apache.org/","https://databricks.com/privacy-policy","https://databricks.com/terms-of-use"] 

	loader = WebBaseLoader(web_links)
	documents = loader.load()

## Splitting in Chunks using Text Splitters


	from langchain.text_splitter import RecursiveCharacterTextSplitter

	text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=20)
	all_splits = text_splitter.split_documents(documents)


## Creating Embeddings and Storing in Vector Store


	from langchain.embeddings import HuggingFaceEmbeddings
	from langchain.vectorstores import FAISS

	model_name = "sentence-transformers/all-mpnet-base-v2"
	model_kwargs = {"device": "cuda"}

	embeddings = HuggingFaceEmbeddings(model_name=model_name, model_kwargs=model_kwargs)

	# storing embeddings in the vector store
	vectorstore = FAISS.from_documents(all_splits, embeddings)


## Initializing Chain


	from langchain.chains import ConversationalRetrievalChain

	chain = ConversationalRetrievalChain.from_llm(llm, vectorstore.as_retriever(), return_source_documents=True)


# QA 测试

	chat_history = []

	query = "What is Data lakehouse architecture in Databricks?"
	result = chain({"question": query, "chat_history": chat_history})

	print(result['answer'])
	
输出

![LLAMA](/img/2023/colab/output-1.webp "all")

	chat_history = [(query, result["answer"])]

	query = "What are Data Governance and Interoperability in it?"
	result = chain({"question": query, "chat_history": chat_history})

	print(result['answer'])

![LLAMA](/img/2023/colab/output-2.webp "all")

查看出处

	print(result['source_documents'])

![LLAMA](/img/2023/colab/output-3.webp "all")



# 备注

## 进入目录

	%cd /content/text-generation-webui
	!echo "dark_theme: true" > /content/settings.yaml
	
## login

	!pip install transformers torch accelerate

	!huggingface-cli login

	!huggingface-cli whoami

## lfs 

git lfs install
git clone https://huggingface.co/meta-llama/Llama-2-7b


## 查看显卡

!nvidia-smi

## pip

	!pip install -Uqqq pip
	!pip install -qqq bitsandbytes==0.40.0
	!pip install -qqq torch==2.0.1
	!pip install -qqq transformers==4.31.0
	!pip install -qqq accelerate==0.21.0
	!pip install -qqq xformers==0.0.20
	!pip install -qqq einops==0.6.1
	!pip install -qqq huggingface-hub==0.16.4
	!pip install -qqq sentencepiece==0.1.99


## improt

	import torch
	from huggingface_hub import notebook_login
	from transformers import GenerationConfig, LlamaForCausalLM, LlamaTokenizer

## login notebook

	notebook_login()

## 模型名字

	MODEL_NAME = "meta-llama/Llama-2-7b-chat-hf"
	tokenizer = LlamaTokenizer.from_pretrained(MODEL_NAME)

	model = LlamaForCausalLM.from_pretrained(
		MODEL_NAME,
		return_dict=True,
		load_in_8bit=True,
		torch_dtype=torch.float16,
		device_map="auto",
	)
