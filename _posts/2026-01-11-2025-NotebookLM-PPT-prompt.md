---
layout:     post
title:      NotebookLM PPT 提示词
subtitle:   NotebookLM slide prompt
date:       2026-01-11
author:     shake
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - AI
---

用AI做PPT，我也算是熟手，以前使用天工，做出很炫酷的PPT。不过这次折腾NotebookLM的ppt，倒是比较费劲。看了一堆的视频，还是没解决自己要做ppt，如何下手。

今天总算摸出了一点门道，记录一下。我已经使用教师风格，乔波斯风格，游戏风格（我的世界）生成的PPT。

NotebookLM，在slide，编辑，填入提示词就可以。

## 传统风格提示词

通过下面提示词，其实是针对不同的场景，你根据自己的需求来选择就可以。

下面的提示词，有5种的使用场景
* 企业
* 儿童
* 漫画
* 教育
* 游戏

相同的内容，你可以使用下面5种风格的提示词。如果你想更换风格，
* 游戏（提示词5），我想改成我的世界，那么把提示词发给AI，让他改成游戏我的世界就可以。
* 教育的内容，就使用提示词4，非常不错。背景就是黑板

```
【提示詞 1｜高品質企業質感簡報】（企业）
創建一個高品質的抽象背景簡報，使用深藍色和金屬金色漸層作為主色調，融入微妙的幾何箭頭紋理來增加視覺深度，添加磨砂玻璃覆蓋層以營造半透明的現代感，右側設計深負空間來平衡布局，提供電影般的燈光效果，包括柔和的陰影和高光，讓整體呈現時尚企業美學，支持高解析度輸出，採用最小主義設計原則，避免過多元素，每張幻燈片保持乾淨簡潔，比例為 16:9，適合橫向投影，並確保文字與圖形完美對齊。

【提示詞 2｜黏土定格動畫風簡報】（儿童）
創建一個完整的黏土定格動畫風格簡報，讓每張幻燈片呈現活潑的黏土模型效果，包括色彩豐富的黏土材質（如紅、藍、綠等鮮豔顏色）、生動的角色或元素（如跳躍、旋轉或變形的靜態姿態），背景使用柔軟的黏土紋理，文字以手捏黏土字體呈現，每頁包含小靜態序列來解釋內容，確保整體風格可愛且互動性強，支持高解析度輸出。

【提示詞 3｜漫畫敘事風簡報】（漫画）
創建一個完整的漫畫風格簡報，使用白色背景與黑色文字作為基底，每頁以現代日本漫畫風格解釋內容，包括多格漫畫面板布局、角色對話泡泡（圓形或雲形）、動感線條（如速度線或爆發效果）與表情符號（如汗珠或星星），讓敘述更具故事性，顏色使用鮮豔調色板（紅、藍、黃為輔助色），每張幻燈片設計為連續故事流，支持高解析度輸出，並確保圖文比例平衡避免擁擠。

【提示詞 4｜黑板教學風簡報】（老师）
創建一個深綠色黑板風格的完整簡報，使用白色、黃色與粉紅色的手寫粉筆文字，模擬真實教室黑板效果，包括輕微擦拭痕跡、隨意塗鴉元素（箭頭、圈圈）與簡單圖示（星星、勾號），背景紋理模仿粉筆灰塵，文字大小具層次以強調重點，加入黑板邊框與橡皮擦視覺元素，結構包含開頭介紹、主要內容與總結頁，整體風格輕鬆且教育化，支持高解析度輸出，並確保文字清晰可讀。

【提示詞 5｜像素復古遊戲風簡報】（游戏）
創建一個像素藝術復古遊戲風格的簡報，使用 8 位元色調（紅色、藍色、綠色）與方塊像素元素，每張幻燈片模擬老派電玩畫面，包含像素角色、磚塊背景紋理與遊戲圖示（如金幣、跳躍人物），內容以關卡面板形式呈現，文字使用像素字體，加入遊戲邊框與分數欄視覺，營造懷舊且趣味的氛圍，支持高解析度輸出，並確保文字清晰不模糊。
```

## Yaml 提示词

这种提示词，非常复杂，你也只能让AI来帮你完成。理论上各种单位公文的PPT，NotebookLM都是可以生成。我这里就模仿乔布斯的PPT模式。

### 乔布斯风格

对各个细节进行定制。每页不超过12个字。适合产品发布。

下面这个google文档，还有2个例子，我没验证，感兴趣可以去玩玩。

```
# https://docs.google.com/document/d/1hVg21NgnF2qOg81HhWDD9c41Dn8EBFgJmWI0sKAcHts/edit?pli=1&tab=t.0
================
# 核心指令：告訴 AI 它的角色
ai_role_definition:
  role: "Chief Storyteller"
  style_model: "Steve Jobs / Zen Presentation"
  objective: "Transform raw information into a cinematic narrative."

# 1. 生成約束 (硬性規定，防止 AI 做成傳統 PPT)
generation_constraints:
  text_density:
    max_words_per_slide: 12  # 嚴格限制：一頁絕不超過 12 個字
    bullet_points: "BANNED"  # 賈伯斯流大忌：絕對禁止條列式清單
    
  visual_priority:
    images: "dominant"       # 圖片永遠是主角
    text: "supporting"       # 文字只是配角
    
  narrative_flow:
    structure: "linear"      # 線性敘事，不跳躍
    emotional_arc: true      # 必須包含情緒轉折（痛點 -> 救贖）

# 2. 幻燈片原型庫 (Slide Archetypes)
# AI 必須將內容「分類」到以下其中一種版型，不可發明新版型
slide_archetypes:
  
  # 類型 A: 震撼大字 (用於章節轉場或核心概念)
  - type: "zen_statement"
    trigger_rule: "Short, powerful concept or transition."
    layout:
      text_size: "huge"    # 100pt+
      alignment: "center"
      background: "void"   # 純漸層背景，無圖
      
  # 類型 B: 英雄登場 (用於產品展示或具象物體)
  - type: "hero_reveal"
    trigger_rule: "Introducing a product, person, or object."
    layout:
      image_style: "full_bleed_or_centered" # 滿版或置中去背
      text_visibility: "minimal" # 僅顯示名稱
      effect: "reflection"       # 倒影效果
      
  # 類型 C: 超級數據 (用於財報或關鍵指標)
  - type: "big_number"
    trigger_rule: "Specific statistic or financial figure."
    layout:
      number_size: "gigantic" # 200pt+
      caption_size: "small"   # 說明文字要很小
      chart_style: "hidden"   # 盡量不放圖表，直接放數字
      
  # 類型 D: 視覺隱喻 (用於解釋抽象概念)
  - type: "visual_metaphor"
    trigger_rule: "Abstract concept (e.g., speed, security, cloud)."
    layout:
      background_image: "high_quality_photo"
      text_position: "overlay_bottom"
      
  # 類型 E: 敵人與衝突 (用於描述現狀痛點)
  - type: "the_villain"
    trigger_rule: "Describing a problem or competitor."
    layout:
      filter: "grayscale_or_dimmed" # 灰階或壓暗，暗示「過時」
      text_tone: "questioning"      # 帶有質疑語氣

# 3. 視覺樣式定義 (給前端渲染引擎看的)
design_system:
  theme:
    background: "radial-gradient(circle, #444444 0%, #000000 100%)" # 經典舞台聚光燈效果
    font_family: "Helvetica Neue, Roboto, San Francisco"
    font_weight: 
      normal: 300 # Light
      bold: 400   # Regular (賈伯斯流不用真正的 Bold)
    color_palette:
      primary: "#FFFFFF"
      accent: "#3498db" # 賈伯斯藍，或其他單一強調色

# 4. 輸出邏輯 (Mapping Logic)
# 告訴 AI 如何處理輸入的長文
input_processing:
  split_logic: "One thought per slide." # 一個念頭一頁
  simplification: "Remove all adverbs and connecting words." # 刪除所有副詞與連接詞
  data_handling: "Extract the single most important number, discard the rest." # 只留一個最重要的數字
```

### 长辈风格

我没测试过。不过这样的提示词风格，NotebookLM也是可以的。

```
長輩風設定:
  氛圍: 充滿祝福與平靜
  視覺元素:
    背景: 盛開的蓮花
    裝飾: 閃亮亮特效
  文字排版:
    字體: 標楷體
    特效: 彩虹漸層
    邊框: 白色粗邊
  內容規則:
    開頭: 早安！
    結尾: 認同請分享

```

### 历史风格

有时候，需要介绍历史，宗教，需要搞一个风格。让AI参考上面内容，输出一个历史介绍风格的yaml格式。这个风格我验证，还是非常不错。

```
历史讲述风格设定:
  氛围: 庄重而温润，带有敬意与沉思
  视觉元素:
    背景: 淡雅古绢纹理或水墨山水意境（可融入青铜器纹样、简牍、竹简等历史符号）
    装饰: 低调金线勾边、古典回纹或云雷纹点缀，避免过度闪亮，强调典雅
  文字排版:
    字体: 标楷体或宋体
    特效: 柔和暖色渐层（如赭石→米白，或墨黑→深褐）
    边框: 浅褐色或墨色细边，简洁有书卷气
  内容规则:
    开头: "敬启者，今日共话一段往事……"
    结尾: "愿历史之光，照亮前路。若有所感，欢迎传阅共思。"

```
