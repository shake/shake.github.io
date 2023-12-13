---
layout:     post
title:      原始格式LLama2-7B转为huggingface格式
subtitle:   LLaMA2-7B convert to HF model
date:       2023-12-13
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

希望可以从零开始，一步一步进行模型的微调。发现第一步，就是需要拿llama 2的原始模型，进行格式的转换，那么这里就记录格式转换全过程。为什么需要做这样的格式转换，有啥好处，后面有说明。


# 格式转换

将原始格式的LLaMA模型转换为Hugging Face Transformers支持的格式

原始模型包括文件如下

![llama-7B](/img/2023/modelscope/llama-7B.jpg "llama-7B")

你需要调整目录如下

![llama-7B](/img/2023/modelscope/llama-7B-add.jpg "llama-7B")


将原始格式的LLaMA模型转换为Hugging Face Transformers支持的格式:

1. **安装所需库**：
   首先确保你已经安装了`transformers`和`torch`库。如果没有，可以使用以下命令安装：

   ```
   pip install transformers torch
   ```

2. **下载LLaMA权重文件**：
   由于LLaMA的权重文件不能直接从Hugging Face Model Hub下载，你需要按照官方指南或授权流程来获取这些权重。

3. **编写转换脚本**：
   创建一个Python脚本来执行模型权重的转换。这个脚本通常会包含加载原始权重、实例化LLaMA模型配置和模型类、然后将权重加载到模型中。最后，将模型保存为Hugging Face的`.pt`格式。

   下面是一个简化的示例代码（假设你已经有了一个名为`convert_llama.py`的脚本）：

   ```python
   # convert_llama.py
   import torch
   from llama.config_mymodel import MyModelConfig
   from llama.modeling_mymodel import MyModel

   def convert_to_hf_format(input_path, output_path):
       # 加载原始权重
       state_dict = torch.load(input_path)

       # 实例化模型配置
       config = MyModelConfig()

       # 实例化模型
       model = MyModel(config)

       # 转换并加载权重
       model.load_state_dict(state_dict, strict=False)  # 注意：如果模型结构有差异，需要设置strict=False

       # 保存为Hugging Face格式
       torch.save(model.state_dict(), output_path)

   if __name__ == "__main__":
       input_path = "path/to/your/original/llama_weights.pth"
       output_path = "path/to/save/hf_format/model.pth"

       convert_to_hf_format(input_path, output_path)
   ```

4. **运行转换脚本**：
   使用Python运行你的转换脚本：

   ```
   python convert_llama.py
   ```

5. **检查输出**：
   确保转换后的模型权重文件已经成功创建在指定的位置。

6. **使用转换后的模型**：
   你可以像使用任何其他Hugging Face Transformer模型一样使用这个转换后的模型。例如，将其加载到`AutoModel`中：

   ```python
   from transformers import AutoModel

   model = AutoModel.from_pretrained("path/to/save/hf_format/model.pth")
   ```

注意，这里给出的步骤和代码是基于一般的转换过程，具体的实现可能需要根据LLaMA模型的实际结构和权重文件进行调整。如果你遇到问题，建议参考LLaMA的官方文档或者社区中的相关教程。

# 为啥需要格式转

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


