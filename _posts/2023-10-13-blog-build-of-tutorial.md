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
* [Blog代码仓库](https://github.com/Huxpro/huxpro.github.io)
* [作者blog，同时也是Demo](https://huangxuan.me)

细看了作者自己的blog的功能，基本可以满足我的需求。那么就可以开始动手。

---

# 准备

其实还是需要准备工作。

* 一个github的账号，你注册的用户名，这个很关键。我github注册的用户名是**shake**
* 一个域名，这样你可以使用自己的域名访问blog。而不是 shake.github.io.我使用的域名：chenshake.com

最好使用国外注册的域名，避免备案的各种麻烦。

---

# fork仓库

访问 [Blog代码仓库](https://github.com/Huxpro/huxpro.github.io)

对这个仓库的代码进行fork，同时把fork的仓库，改名为 shake.github.io

![修改reop名字](/img/repo-name.jpg "Repo name")

这个时候，如果你直接访问 shake.github.io，其实你会看到404的错误。你还是需要针对新的域名，进行相关的配置。

---

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

---

# 自定义域名支持HTTPS

---

#### 设置DNS

首先我们需要知道 shake.github.io 地址，通过[chinaz ](https://ip.chinaz.com/)

你会发现解析多个IP地址，先记录下来。

登录你的域名提供商，进行设置，这是一个视频截图，他的设置是正确的。

![DNS设置](/img/domain-name.jpg "domain name")

cname 设置，www，指向 shake.github.io 

我是使用namecheap，专门提供的视频设置DNS，实现Enforce HTTPS in GitHub Pages with Namecheap Domain。[视频地址](https://www.youtube.com/watch?v=FBtehan5DAo&ab_channel=WhatMakeArt)

#### 修改配置

修改 repo 根目录下的文件 _config.yml,这次就是修改一下URL


	# Site settings
	title: 陈沙克日志
	SEOTitle: 陈沙克日志 | shake Blog
	header-img: img/home-bg.jpg
	email: shake.chen@gmail.com
	description: "用git来记录生活"
	keyword: "陈沙克, openstack, devops,  出国留学, 清迈, 泰国"
	#url: "https://shake.github.io" # your host, for absolute URL
	url: "https://www.chenshake.com" # your host, for absolute URL
	baseurl: "" # for example, '/blog' if your blog hosted on 'host/blog'
	github_repo: "https://github.com/shake/shake.github.io.git" # you code repository

修改 repo 根目录下的文件 CNAME

	chenshake.com
	
很简单，把域名修改成自己的，就可以。


#### 设置repo强制使用HTTPS

登录github，打开shake.github.io repo的 settings

![强制https](/img/https.jpg "domain name")

这就基本完成了https访问的配置。不出意外，你这时候，就可以使用 https://www.chenshake.com 访问。

---

# GithHub Desktop

这个工具确实非常不错，确实解决github维护blog的问题，完全可以在图形化界面上完成所有的操作。我是在windows机器安装，基本没太多麻烦。就是正常下载，安装，使用就可以。现在存储位置的地方，选择非系统盘就可以。

通过Github Desktop 下载 repo shake.github.io到本地。你就可以进行定制自己的blog。

---

# 清理

我是直接把两个目录：
* img
* _posts

全部删除，其实就是你直接访问同步文件夹，删除就可以。不需要命令行操作那么复杂。

这个时候，Github Desktop就会发现有内容变化，你就只需要 commit，push，就可以了。

# 定制Blog

#### 修改网站的 icon 

在博客 img 目录下找到并替换 favicon.ico 这个图标即可，图标尺寸为32x32。我是使用我的照片，通过[wizlogo](https://wizlogo.com/favicon-generator) 转换，直接放到img目录下就可以了。

#### 修改主页的座右铭

直接在根目录下找到index.html,修改就可以。你完全可以在windows资源管理器下搜索就可以找到修改的地方。

#### 修改配置

主要还是为了提升速度，减少加载的内容。把分析，评论都关闭，朋友链接。
[我的配置](https://github.com/shake/shake.github.io/blob/master/_config.yml)

	# Sidebar settings
	sidebar: true # whether or not using Sidebar.
	sidebar-about-description: "要做一个数字游民"
	#sidebar-avatar: https://github.com/Huxpro.png # use absolute URL, seeing it's used in both `/` and `/about/`
	sidebar-avatar: https://shake.github.io/img/chenshake.jpg
	
记得把你的照片也放到img目录下。

#### 修改About

这个地方同时支持中文和英文，考虑很周到。就是修改两个文件。

[我的修改](https://github.com/shake/shake.github.io/tree/master/_includes/about)

包括如果你对首页的foot底下有什么修改需求，都是可以通过这个includes的目录下相关文件进行修改。



















