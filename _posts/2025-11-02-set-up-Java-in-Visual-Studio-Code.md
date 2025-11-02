---
layout:     post
title:      Vscode 配置java
subtitle:   set up Java in Visual Studio Code
date:       2025-11-02
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AP
    - son
---

昨晚儿子问vscode，无法跑他的java代码。因为以前学习，都是使用web ide。那么用本地的vscode跑，还是第一次。

如何让vscode支持java，刚好我现在已经改成vscode写blog，有环境，就专门验证一遍。

我通常习惯，就是上youtube，找一个合适的视频看完，就差不多，可以动手验证。

vscode，孩子已经装好在电脑上，这个其实就是到官网，直接下载，安装就可以。

关于java的支持，其实官方文档支持还是非常不错的。

[youtube视频](https://www.youtube.com/watch?v=BB0gZFpukJU)

## 本地环境

验证本地环境下，是否安装java。很简单，cmd，进入命令行

```
java --version

```

如果你没安装过，会显示没有。

## 下载安装

官方的文档，提供的下载的地址。

[vscode 官方文档安装java](https://code.visualstudio.com/docs/languages/java)

选择操作系统win or mac。


![web](/img/2025/nov/web.png "web")

![java-check](/img/2025/nov/java-check.png "java-check")

![java-install](/img/2025/nov/java-install.png "java-install")


## hello world

验证环境是否可用

命令行下，

```
java --version

```

![java-version](/img/2025/nov/version.png "java-version")


hello world

```
    public class HelloWorld {
        public static void main(String[] args) {
            System.out.println("Hello World!");
        }
    }

```

## 修改jdk版本

当你运行java代码到时候，可能会出现update sdk提示

![update-jdk](/img/2025/nov/update-jdk.png "update-jdk")

如果你选择后，还想进行修改，可以在java 项目里，进行配置


![config-jdk](/img/2025/nov/config-jdk.png "config-jdk")

