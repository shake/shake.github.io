---
layout:     post
title:      NotebookLM Infographic 提示词
subtitle:   NotebookLM Infographic prompt
date:       2026-01-11
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

最近google的infographic 图，非常火爆，因为展示的内容非常复杂和震撼。要达到自己想要的效果，或者让提示词具有通用性。这个我也折腾的好久。直到找到自己的需求。这样就很快掌握了。

孩子预计今天要考雅思。那么我想要给他准备啥，需要大概率用不上。但是确实可以用这个来搞一遍。youtube，找到最热门的雅思博主，粉丝超过100万以上才能考虑。其实就那么4个，只选择2026年最新雅思内容的视频。

大概有4种的infographic，至少目前我看到的

* Handwritten,手写笔记
* hand-drawn sketchnote。手绘的图
* 海报
* 产品说明书
  

选择不同的生图提示词，相同的内容就会生成不同风格的图。

## NotebookLM

放进去了13的视频。我没有通过聊天生成单独的文本，作为源，单独生成图片，这个更加精准，也是很有道理。但是google的智商肯定比我们高，他知道我们的需求，不需要那么麻烦。直接提出你的需求就可以。

![提示词修改](/img/2026/jan/info.png "修改提示词")

### 听力

下面的提示词是老外视频翻出来的。

```
List the most crucial information about [topic] that [audience] mustknow. Do not change the title. Be specific and explanatory. Usead smoothly, provide any requiredcomplete sentences that are redefinitions and give examplesoillustration(s) that can be used to explainthe key concept(s) of each section.Do not create a 'Why it matterssection, Limit section titles to the absolute minimum.
```
我让ai翻译成中文，我根据自己需求

* [topic]，改成 [雅思听力考试]
* [audience] 改成 考生


```
列出[雅思听力考试]中最关键的信息，这些信息是考生必须了解的。请勿更改标题。内容要具体且具有解释性。行文流畅，提供必要的完整句子，包括重新定义和示例或插图，以解释每个部分的关键概念。请勿创建“重要性”部分，并将章节标题限制在绝对必要的范围内。
```
相同的提示词，如果生成英文，有时候内容，更加丰富。

![雅思听力重点](/img/2026/jan/listen.png "雅思听力")

### 口语

![雅思口语重点](/img/2026/jan/speak.png "雅思口语")

### 写作

做了一个调整，改成详细内容

![info](/img/2026/jan/detail.png "info")

雅思的写作分成task1和task2. 选择detail的情况下，一张图无法覆盖全部内容

![task2](/img/2026/jan/task2.png "task2")

由于我的资料里关于task1太少，采用详细模式，无法生成对应的图，改成 **标准** 才行

![task1](/img/2026/jan/task1.png "task1")

### 阅读

我还是选择 **detail** ，看看生成的内容。

![read](/img/2026/jan/read.png "read")


## Gemini 生成Infographic

可以通过Gemini，生成内容，再生成Infographic。需要采用Thinking 模式。

### Handwritten,手写笔记

#### 内容提示词

在think模式下，提交下面的提示词。

```
List the most crucial information about [topic] in Educationthat teachers must know. Do not change the title. Be specific and explanatory. Use completesentences that are read smoothly, provide any required definitions and give examples ofilustration(s) that can be used to explain the key concept(s) of each section. Do not create a'Why it matters' section. Limit section titles to the absolute minimum.
```

可以翻译成中文，方便很多。例如我想生成埃及文明的内容。把topic，改成埃及文明。

```
列出教师必须了解的关于埃及文明在教育中的教学影响的最关键信息。内容需具体、解释清晰，使用流畅完整的句子，提供必要的定义，并辅以示例或说明，以阐明每个部分的核心概念。各部分内容标题应简洁明了，不另设“重要性”部分。
```

#### 生图提示词

添加工具，**image**

提示词如下,只需要修改一下显示的比例，我习惯，16:9，就不需要修改。

```
Create an educational infographic on aged, lined, spiral-bound noteboopaper with a [16:9] ratio, based on the above information. The visualaesthetic must use realistic, detailed colored pencil and watercolortextures for illustrations alongside neat, printed architectural-stylehandwriting for text, Design the page with a decorative main title bannat the top, followed by the most fundamental definition and a large,cinematic panoramic illustration spanning the width of the page. Belowthis, organize key concepts into a bulleted list section using stylizedcheckmark icons, and divide the remaining lower half of the page intoa modular grid of distinct, rectangular boxes, where each box containsspecific, fully labeled comparative diagram or classification illustrationwith its own descriptive caption, Use no titles other than the main one.
```

中文

```
基于上述内容，创建一张以泛黄、带横线、螺旋装订笔记本纸为背景的教育信息图，比例为[16:9]，视觉风格须采用逼真细腻的彩色铅笔与水彩质感插图，并搭配整洁印刷体般的建筑风格手写字体用于文字，页面顶部设计装饰性主标题横幅，其下紧接最核心的定义及一幅横贯全页宽度的电影感全景插图，再下方以风格化对勾图标引导的项目符号列表呈现关键概念，页面下半部分则划分为模块化网格，由多个独立矩形框组成，每框内含一幅具体且完整标注的对比图或分类图并配以说明性图注，除主标题外不得使用其他任何标题。
```

看图。在google gemini，提示词中文和英文是没区别。显示内容，中文，英文会有点差异。英文更加丰富。

![gemini](/img/2026/jan/gemini.png "gemini")


### hand-drawn sketchnote。手绘的图

Gemini think模式，输入

#### 内容提示词

```
 #Identify and explain [insert your topic]. Be specific and to the point. Provide relevant examples. Audience is [define the sketchnote's audience]

Identify and explain Anger Management Techniques. Be specific and to the point. Provide
relevant examples. Audience is 10 year old students.

Identify and explain children and parents to get along well Techniques. Be specific and to the point. Provide relevant examples. Audience is 10 year old students.

```
中文

```
讲解并解释孩子和父母如何才能和睦相处的技巧。内容要具体、简洁明了。提供相关的例子。目标受众是10岁的学生。
```

#### 生图提示词

直接输入下面提示词就可以。

```
Create a hand-drawn sketchnote visual summary of these notes, Use apristine white paper background (no lines). The art style should be'graphic recording' or 'visual thinking' using black ink fine-liners forclear outlines and text, Use colored markers (specifically teal, orangeand muted red) for simple shading and accents, Center the main titlein a 3D-style rectangular box, Surround the title with radially distributedsimple doodles, business icons, stick figures, and graphs that explainthe concepts, Use arrows to connect ideas, The text should bedistinct, handwritten, all-caps printing, legible and organized like aprofessional brainstorming session. 16:9

```

![sketchnote](/img/2026/jan/sketchnote.png "sketchnote")

### 海报

#### 视频总结提示词

```
Summarise this video: https:/ww.youtube.com/watch?v=5a-9ccPDibU . Be brief and to the point. Audience is Al Enthusiasts。
```

图片生成，提示词就是上面那段，无需修改，记得启用 **图片**。

![video](/img/2026/jan/video.png "video")


#### 海报生图提示词


```
create an inforgraphic based on the above information, accompainied by photorealistic, appropriate for the general populous,9:16

根据以上信息制作一张信息图，并配以逼真的图片，内容适合大众，比例为 9:16。

```

![poster](/img/2026/jan/poster.png "poster")


### 产品说明书

#### 生图提示词
这个比较复杂，人体说明，电器说明，都可以。

```
“Create an infographic image of [Garmin 255], combining a realistic photograph or photoreal render of the object with technical annotation overlays placed directly on top.

Use black ink–style line drawings and text (technical pen / architectural sketch look) on a pure white studio background, including:

•Key component labels

•Internal cutaway or exploded-view outlines

•Measurements, dimensions, and scale markers

•Material callouts and quantities

•Arrows indicating function, force, or flow (air, sound, power, pressure)

•Simple schematic or sectional diagrams where relevant

Place the title Garmin 255 inside a hand-drawn technical annotation box in one corner.

Style & layout rules:

•The real object remains clearly visible beneath the annotations

•Annotations feel sketched, technical, and architectural

•Clean composition with balanced negative space

•Educational, museum-exhibit / engineering-manual vibe

Visual style:

Minimal technical illustration aesthetic, black linework over realistic imagery, precise but slightly hand-drawn feel.

Color palette:

White background, black annotation lines and text only. No colors.

Output:

1080×1080, ultra-crisp, social-feed optimized, no watermark.”

```

![watch](/img/2026/jan/watch.png "watch")