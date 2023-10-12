---
layout:     post
title:      Blog搭建过程
subtitle:   希望可以能帮上你
date:       2023-10-13
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - 技术
---

这两天已经把blog设置到自己的理想状态，如果现在不记录，那么应该很快就会忘记。所以为了自己日后维护，还是要记录一下这个过程。由于技术的变化，其实参考一篇文档，你很难全部搞定，随着时间的推移，越来越不靠谱。这次就是把我自己的经历，记录一些。

参考文章： [博客搭建详细教程](https://github.com/qiubaiying/qiubaiying.github.io/wiki/%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA%E8%AF%A6%E7%BB%86%E6%95%99%E7%A8%8B)。

文章内容很详细，操作不同的地方，我会重点强调。

---
# 调研

Blog搭建的代码是由 Huxpro 开发和维护，
* [作者github地址](https://github.com/huxpro) 
* [Blog代码仓库] (https://github.com/Huxpro/huxpro.github.io)
* [作者blog，同时也是Demo]（https://huangxuan.me）

细看了作者自己的blog的功能，基本可以满足我的需求。那么就可以开始动手。

# 准备

其实还是需要准备工作。

* 一个github的账号，你注册的用户名，这个很关键。我github注册的用户名是**shake**
* 一个域名，这样你可以使用自己的域名访问blog。而不是 shake.github.io.我使用的域名：chenshake.com

最好使用国外注册的域名，避免备案的各种麻烦。

# fork仓库

访问 [Blog代码仓库] (https://github.com/Huxpro/huxpro.github.io) 

对这个仓库的代码进行fork，同时把fork的仓库，改名为 shake.github.io

![修改reop名字](img/repo-name.jpg "Repo name")

这个时候，如果你直接访问 shake.github.io，其实你会看到404的错误。你还是需要针对新的域名，进行相关的配置。

# 修改配置

修改 repo 根目录下的文件 _config.yml


	# Site settings
	title: 陈沙克日志
	SEOTitle: 陈沙克日志 | shake Blog
	header-img: img/home-bg.jpg
	email: shake.chen@gmail.com
	description: "用git来记录生活"
	keyword: "陈沙克, openstack, devops,  出国留学, 清迈, 泰国"
	url: "https://shake.github.io" # your host, for absolute URL
	#url: "https://www.chenshake.com" # your host, for absolute URL
	baseurl: "" # for example, '/blog' if your blog hosted on 'host/blog'
	github_repo: "https://github.com/shake/shake.github.io.git" # you code repository

参考修改，提交。这个时候，就应该可以通过 shake.github.io 进行访问。



