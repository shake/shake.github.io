---
layout:     post
title:      youtube2mp3
subtitle:   手工下载youtube视频转换mp3
date:       2025-08-05
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

我想做一个应用，可以直接下载youtube的视频，转换成mp3. 在现在的AI code时代，大家都能想到，如何用ai，把这个应用搞出来。

上次朋友说，AI code 是否真的能打，现实中使用。搞些小游戏，这种只适合demo。需要真的解决自己的问题，才能真的靠谱。

这个需求，其实是来自儿子。现在如果直接下载mp3，已经是非常麻烦的事情。你从腾讯音乐，酷狗，下载的歌曲，都是数字版权保护。当然你是可以用工具搞定，但是我日常听歌都是来自youtube。自然想到，是否可以下载youtube的歌曲视频，转换成mp3.

是否能打包成一个app，应用，可以在win11下使用。输入youtube的链接，就可以马上转换成mp3.

我具体操作起来，其实就遇到了不少麻烦。想一步到位。还是比较困难的。需要进行拆解。

我的理解，第一阶段：

* macos 终端调试通过，yt-dlp，实现下载。
* macos ffmpeg 转换成mp3.


完成这些活，看起来简单，操作起来其实遇到一堆问题。下面记录一下。

## 软件

* yt-dlp
* ffmpeg

## 获取youtube的cookies

通过chrome的插件，“Get cookies.txt LOCALLY”扩展程序。获取了youtube的cokeries。导出一个text文件。

```
yt-dlp --cookies ~/Downloads/cookies.txt -o ~/Downloads/阿細《赤的疑惑》.mp4 "https://www.youtube.com/watch?v=GNnGlZQmP7Q"

```

这样你才能正常下载youtube的资源。

```
# 高质量的音频和视频
yt-dlp -f bestvideo+bestaudio -o ~/Downloads/阿細《赤的疑惑》.mp4 https://www.youtube.com/watch?v=GNnGlZQmP7Q

```

下载回来的文件后缀是：webm. 以前没留意过。这次也专门问了一下AI。这是什么格式。

就是最新的视频和音频的格式。压缩高。

转换成MP3 音频

```
# 320kbps，和所谓无损音乐一样效果。
ffmpeg -i 阿細《赤的疑惑》.mp4.webm -vn -ab 128k 阿細《赤的疑惑》.mp3

```

-i ：指定输入文件。
-vn ：禁用视频录制（因为我们只需要音频）。
-ab ：设置音频比特率（单位是 kbps）。128k 是常见的比特率值，数值越大，音频质量越高，文件体积也越大。你可以根据需要调整这个值，比如 64k、192k、320k 等。

后续要解决就是如何在macos下，提供一个图形界面，输入链接，就可以出来mp3.