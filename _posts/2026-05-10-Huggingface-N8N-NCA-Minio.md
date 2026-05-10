---
layout:     post
title:      Hugging Face Spaces 容器部署 n8n + MinIO + NCA
subtitle:   n8n + MinIO + NCA Toolkit
date:       2026-05-10
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

在有AI协助的年代，部署，安装一个应用，应该是很简单的事情。但是有时候的组合是你第一个的时候，需要踩坑，还是非常多。估计这个是我最近花时间最做的填坑的经历。

* [n8n](https://github.com/n8n-io/n8n)
* [supabase](https://github.com/supabase/supabase)
* [minio](https://github.com/minio/minio)
* [
no-code-architects-toolkit](https://github.com/stephengpope/no-code-architects-toolkit)
* [HF Spaces](https://huggingface.co/spaces)
  
我自己的空间,可以直接使用。

* [N8N](https://huggingface.co/spaces/chenshake/n8n)
* [Minio](https://huggingface.co/spaces/chenshake/minio)
* [no-code-architects-toolkit](https://huggingface.co/spaces/chenshake/no-code-architects-toolkit)


## 什么是 Hugging Face Spaces？

Hugging Face（社区简称 **HF**）是全球知名的 AI 开源社区。其推出的 **Spaces** 功能，为开发者提供了一站式的模型部署与演示平台，让创意快速落地、成果轻松分享。

---

###  免费算力资源，开箱即用
- **配置规格**：4 核 CPU / 16GB 内存 / 1GB 存储（免费层级）
- **零门槛启动**：无需配置服务器，上传代码即可运行
- **适合场景**：学习实践、原型验证、小型应用部署

###  海量示例，一键复用
- 浏览社区中丰富的 Spaces 项目，涵盖文本、图像、音频等多模态应用
- 支持「Duplicate Space」一键复制，快速基于他人项目二次开发
- 降低试错成本，加速学习与创新迭代

###  自带域名 + HTTPS，解决访问痛点
- 每个 Space 自动分配 `*.hf.space` 专属域名
- 默认启用 HTTPS 加密，满足现代浏览器安全策略
- 无缝集成 Docker/Gradio/Streamlit 等框架，免去反向代理与证书配置烦恼

### 复制项目

例如访问我的项目：[n8n](https://huggingface.co/spaces/chenshake/n8n)

选择 **Duplicate this Space**

![复制repo](/img/2026/may/hf01.png "复制repo")



## N8N+Supabase

HF Space上搭建N8N，需要解决的问题就是存储问题。目前其实HF提供Bucket 存储给Docker使用，可以解决很多问题。这次还是使用的外部的数据库的方式来解决。

Supabase，是一家PG数据库开源厂商，非常火爆，现在开源的一个玩法，就是提供一个SaaS服务。这个SaaS服务，有免费的版本，可以满足N8N需求。

参考视频 [n8n 上雲端只能付費?打造免費雲端主機終極指南含永不休眠祕訣](https://www.youtube.com/watch?v=dAC6kpVvryQ&list=PLSDw3sVdV2I7TwTx5BTTFLixkd1-reDCl&index=2)

视频介绍了全过程。了解一下原理就可以，有更加简单的版本，就是这Space，搜索N8N，看到相同的的部署，直接复制过来。唯一的小门槛就是填写环境变量。




Spaces 搜索：N8N，你会看到点赞最多的，就一个，很好选择

[点赞最多，推荐](https://huggingface.co/spaces/baoyin2024/n8n-free)

N8N,这样就基本搞定.

```
# HF有两个地址，需要区分

# 项目地址
# 项目访问地址：http://huggingface.cn/spaces/(用户名)/(创建应用空间名字)
https://huggingface.co/spaces/chenshake/n8n

# 部署应用的访问地址，
# 对于n8n来说，这是控制台地址，登陆就可以，n8n，只需要一个访问端口就可以，比较简单。
# 应用访问地址：https://(用户名)-(spaces 创建应用空间名字).hf.space
https://chenshake-n8n.hf.space

```

### 健康检查

```
curl -I https://chenshake-n8n.hf.space

# 输出内容
HTTP/2 200
date: Sun, 10 May 2026 12:08:45 GMT
content-type: text/html; charset=utf-8
content-length: 16993
accept-ranges: bytes
cache-control: public, max-age=0
last-modified: Fri, 01 May 2026 16:43:13 GMT
etag: W/"4261-19de46c42dd"
vary: Accept-Encoding
vary: origin, access-control-request-method, access-control-request-headers
x-proxied-host: http://10.111.121.71
x-proxied-replica: upr4qe74-ls49x
x-proxied-path: /
link: <https://huggingface.co/spaces/chenshake/n8n>;rel="canonical"
x-request-id: oZM_Fr
access-control-allow-credentials: true

```

### 环境变量设置



```
## Space secrets

# Supaabse 创建数据库的密码

DB_POSTGRESDB_PASSWORD

# Supaabse 用户

DB_POSTGRESDB_USER

# 这是N8N用来加密代码，你只需要输入你的密码就可以。
N8N_ENCRYPTION_KEY

# 这是上面视频使用的参数，做健康检查使用，其实健康检查是不需要验证, 任意填写或者删掉
N8N_BASIC_AUTH_USER
N8N_BASIC_AUTH_PASSWORD

## Space variables，根据自己情况调整

# 默认：Asia/Shanghai

GENERIC_TIMEZONE

# TZ，默认：Asia/Shanghai

TZ

# 只能是pg：postgresdb
DB_TYPE

# 默认：public，无需更改

DB_POSTGRESDB_SCHEMA

# 这个需要根据你在Supabase cloud创建的地址

* DB_POSTGRESDB_HOST
* DB_POSTGRESDB_PORT


# n8n 端口，7860，无需更改，不要改，因为这个端口需要和repo的README.md 对应

N8N_PORT

# https，这就是HF spaces优势，提供https

N8N_PROTOCOL

# 3个地址相同，https://(username)-(project name).hf.space,https://chenshake-n8n.hf.space

* N8N_EDITOR_BASE_URL
* WEBHOOK_URL
* N8N_HOST

# 默认：false

N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS

# 默认： true

NOTION_MARKDOWN_CONVERSION

# 默认： 0，
NODE_TLS_REJECT_UNAUTHORIZED

# 默认： true

# 默认 72，我理解是72小时，3天，就是一个工作流，最长运行时间

EXECUTIONS_DATA_MAX_AGE

# 默认：all，保持错误日志

EXECUTIONS_DATA_SAVE_ON_ERROR

# 默认：none，成功的日志不保留

EXECUTIONS_DATA_SAVE_ON_SUCCESS



```


## Minio

我是部署NCA的过程，发现绕不过S3存储，代码要求不能使用本地存储，只能使用S3兼容或者google 存储。所以就必须先解决NCA存储的问题。

minio在space上的搜索，数量不多。还是非常多坑。

* minio，需要2个端口，一个后台，一个api。但是HF，只能有一个入口，所以必须用nginx
* 官方镜像是一个精简的版本。你必须使用红帽打包的镜像才行：quay.io/minio/minio
  
```
# HF有两个地址，需要区分

# 项目地址
https://huggingface.co/spaces/chenshake/minio

# 部署后的地址，对于minio来说，这是控制台地址，也是api地址，无需端口

https://chenshake-minio.hf.space/

```

登陆minio 控制它，创建一个bucket：nca-toolkit，设置public。这是后续需要使用。


### Minio 健康检查

```
curl -I https://chenshake-minio.hf.space/minio/health/live

# 输出内容
HTTP/2 200
date: Sun, 10 May 2026 12:07:59 GMT
content-length: 0
server: nginx/1.22.1
x-xss-protection: 1; mode=block
accept-ranges: bytes
strict-transport-security: max-age=31536000; includeSubDomains
vary: Origin
vary: origin, access-control-request-method, access-control-request-headers
x-amz-id-2: dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8
x-amz-request-id: 18AE32E926A7B6F5
x-content-type-options: nosniff
x-proxied-host: http://10.111.137.195
x-proxied-replica: u1n1lngi-27lb7
x-proxied-path: /minio/health/live
link: <https://huggingface.co/spaces/chenshake/minio>;rel="canonical"
x-request-id: wJT4nf
access-control-allow-credentials: true

```

## 环境变量

```
# Space secrets

## MINIO_ROOT_PASSWORD, 就是登陆的密码。用户名：admin，dockerfile，可以看到。这里输入你自己的密码

MINIO_ROOT_PASSWORD

Space variables

# MINIO_BROWSER_REDIRECT_URL很关键，通过nginx设置实现，必须填写：https://chenshake-minio.hf.space/browser/，根据自己情况修改
MINIO_BROWSER_REDIRECT_URL

```
### 备注

我定制了一个html页面，为了优化体验。如果你使用，估计需要修改页面的一个链接地址。


## NCA （no-code-architects-toolkit）

No-Code Architects Toolkit 是一款100%免费的开源API工具（Python/Flask构建），提供音频转录、视频字幕、媒体格式转换、云存储管理等媒体处理功能，支持Docker一键部署，可替代Whisper、CloudConvert等付费服务，适合无代码开发者与自动化团队使用。

* Spaces搜索，发现就只有一个，使用官方镜像，这是大坑，官方镜像是一个精简版，无法使用
* 镜像上需要自己使用官方代码，build镜像才行
* 把代码git到本地，上传HF，发现HF对小文件，图片，要求必须用LFS，
* 使用官方代码里的Dockerfile，还需要修改：README.md, 这个是HF特点，通过README.md 进行端口设置
* app.py 加入健康检查的路由和一个简单的优化显示

### 健康检查

```
curl -I https://chenshake-no-code-architects-toolkit.hf.space/health

## 输出内容
HTTP/2 200
date: Sun, 10 May 2026 12:07:02 GMT
content-type: text/html; charset=utf-8
content-length: 2
server: gunicorn
x-proxied-host: http://10.111.132.104
x-proxied-replica: dipl8pwj-vc7nj
x-proxied-path: /health
link: <https://huggingface.co/spaces/chenshake/no-code-architects-toolkit>;rel="canonical"
x-request-id: QzRKug
vary: origin, access-control-request-method, access-control-request-headers
access-control-allow-credentials: true

```

### 环境变量设置

```
## Space secrets

# API KEY，其实是NCA访问密码，Space secrets，设置你自己个人密码就可以

API_KEY

# Minio的 access key，这个登陆mino后台，创建就可以

S3_ACCESS_KEY

# Minio的密钥，填入就可以。

S3_SECRET_KEY

## Space variables，都会有默认数值，

# api请求地址，已经经过nginx的代理，调整成你自己就可以：https://chenshake-minio.hf.space

S3_ENDPOINT_URL

# 这个是需要在minio，创建一个bucket nca-toolkit，设置public，才行
S3_BUCKET_NAME

# Region，是为了兼容s3，照抄就可以：us-east-1
S3_REGION

#直接默认 /tmp
LOCAL_STORAGE_PATH

# 并发数量，为了避免内存爆，数字：2，比较保险。
GUNICORN_WORKERS

```
