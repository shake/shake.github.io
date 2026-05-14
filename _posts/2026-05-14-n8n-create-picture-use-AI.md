---
layout:     post
title:      N8N HTTP Request cURL倒入的坑
subtitle:   n8n workflow
date:       2026-05-14
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

N8N的HTTP Request 节点，有一个功能，通过cURL倒入，非常方便，对于非程序员来说，友好很多，但是有两个坑，需要注意一下。

* curl倒入，代码没有嵌套，N8N，使用field，如果代码有嵌套，使用JSON，
* 使用field，有的字段会出现问题。下面解析。

尽量避免使用filed，手工使用JSON

## Minimax 生图

一个手动出发器，连接一个 HTTP Request 节点，就可以实现。问题就是HTTP Request 节点如何设置

打开Minimax的文档中心，[API参考](https://platform.minimaxi.com/docs/api-reference/image-generation-t2i)

文生图

可以看到右边有一个curl 例子，非常实用，直接copy就可以。填入自己的key。key一定要注意，前面有**Bearer**


```
curl --request POST \
  --url https://api.minimaxi.com/v1/image_generation \
  --header 'Authorization: Bearer <token>' \
  --header 'Content-Type: application/json' \
  --data '
{
  "model": "image-01",
  "prompt": "A man in a white t-shirt, full-body, standing front view, outdoors, with the Venice Beach sign in the background, Los Angeles. Fashion photography in 90s documentary style, film grain, photorealistic.",
  "aspect_ratio": "16:9",
  "response_format": "url",
  "n": 3,
  "prompt_optimizer": true
}
'

```

n8n里打开HTTP Request节点，选择 import cURL，将上面内容贴入。建议你先在文本里，输入自己的key，再倒入。

![ HTTP Request 节点](/img/2026/may/n8n-01.png "HTTP Request")

倒入参数后，结果你发现点击执行，出错。这时候就郁闷。

## 方法1

修改参数，原因是因为

以下是为您整理的 Markdown 表格格式（已优化排版，并将 `n8n` 表达式和实际发送值用代码高亮以便阅读）：

| 填入内容 | n8n Field 模式实际发送 | 结果 |
| :--- | :--- | :--- |
| `3` | `"3" (String)` | 报错 (如果 API 要求 Integer) |
| `true` | `"true" (String)` | 报错 (如果 API 要求 Boolean) |
| &#123;{ 3 }&#125;
 | `3 (Number)` | 正常 (通过表达式强制声明类型) |
| `{{ true }}` | `true (Boolean)` | 正常 (通过表达式强制声明类型) |

> 💡 **注**：原文本中的 `"""3"" (String)"` 为 CSV 转义格式，此处已还原为标准的 `"3" (String)` 以便在 Markdown 中更清晰地展示。可直接复制上方表格使用。

调整完成就正常。

## 方法2

把 Using Fields Below，改成 Using JSON，输入下面内容

```

{
  "model": "image-01",
  "prompt": "A man in a white t-shirt, full-body, standing front view, outdoors, with the Venice Beach sign in the background, Los Angeles. Fashion photography in 90s documentary style, film grain, photorealistic.",
  "aspect_ratio": "16:9",
  "response_format": "url",
  "n": 3,
  "prompt_optimizer": true
}
```

填入你的key，都可以工作正常。


## 参考

阿里云百炼的模型，curl，是嵌套，倒入直接using JSON

[阿里百炼平台](https://bailian.console.aliyun.com/cn-beijing?spm=a2ty02.46012040.0.0.58d57c29IZQjFp&tab=api#/api/?type=model&url=3002354)



