---
name: minimax-image
description: MiniMax image-01 文生图技能。通过「叙事层→氛围层→技术层」三层引导框架，帮你一步步构建精准 Prompt 并生成图片。
trigger:
  - "minimax"
  - "文生图"
  - "画一张图"
  - "text to image"
process: mandatory
---

# MiniMax 文生图技能

## ⚠️ 强制流程（禁止跳过）

**触发后必须按顺序执行：**
1. 先询问用户需求（Layer 0-1）
2. 逐步引导收集信息（Layer 1-2）
3. 汇总确认 Prompt（Layer 3-4）
4. 才能调用 API

**禁止：未经引导直接生图**

---

## 三层引导框架

```
Layer 0 → Layer 1（叙事层）→ Layer 2（氛围层）→ Layer 3（技术层）→ Step 4 汇总 → API
```

---

## Layer 0 — 入口

**触发后第一句话（必须问）：**

> 「这张图用于什么场景？想画什么？」

收集到回答后，才能进入 Layer 1。

---

## Layer 1 — 叙事层

### Step 1.1 图像用途

| # | 选项 | 说明 |
|---|------|------|
| A | 社媒配图 | 小红书/X/抖音/Instagram |
| B | 产品展示 | 电商/广告/商品图 |
| C | 头像/形象照 | 个人品牌/Profile |
| D | 内容插图 | 公众号/博客/PPT |
| E | 视频封面 | YouTube/短视频封面 |
| F | 艺术收藏 | 家居装饰/限量版画 |
| X | **自定义** | 用户描述 |

---

### Step 1.2 主体类型

| # | 选项 | 说明 |
|---|------|------|
| A | 人物 | 人像/模特/肖像 |
| B | 产品/物品 | 商品/实物/产品 |
| C | 场景/风景 | 建筑/自然/环境 |
| D | 动物 | 宠物/野生动物 |
| E | 抽象/概念 | 情绪/想法/隐喻 |
| X | **自定义** | 用户描述 |

---

### Step 1.2A — 女生人像摄影（主线，7个维度）

**1.2A-1 数量**
| # | 选项 |
|---|------|
| A | 单人 |
| B | 双人/情侣/闺蜜 |
| C | 多人（3-5人） |
| D | 背影/剪影 |
| X | 自定义 |

**1.2A-2 容貌风格/地域感** ⭐
| # | 选项 | 说明 |
|---|------|------|
| A | **韩系** | 精致、骨感、淡妆、轻欲风 |
| B | **日系/厌世脸** | 清冷、淡颜系、高级感、无辜眼 |
| C | **新疆/异域感** | 深邃五官、浓颜系、立体轮廓 |
| D | **中式/东方美** | 温婉、鹅蛋脸、古典气质 |
| E | **泰系/混血感** | 浓颜系、高鼻梁、深邃眼窝 |
| F | **法式/慵懒** | 随性、自然、雀斑可 |
| G | **明星脸/精致五官** | 五官精致、接近公众审美 |
| H | **无特定/自然真实** | 不追求特定风格 |
| X | **自定义** | 用户描述 |

**1.2A-3 肤色/质感**
| # | 选项 |
|---|------|
| A | 白皙/冷白皮 |
| B | 暖白/奶油肌 |
| C | 自然健康肤色 |
| D | 小麦色/古铜色 |
| E | 无特定/自然质感 |
| X | 自定义 |

**1.2A-4 妆容风格**
| # | 选项 |
|---|------|
| A | 素颜/伪素颜 |
| B | 清透/裸妆感 |
| C | 精致全妆 |
| D | 复古妆/港风 |
| E | 欧美妆/轻欲 |
| X | 自定义 |

**1.2A-5 发型/发色**
| # | 选项 |
|---|------|
| A | 长发/披肩 |
| B | 短发/锁骨发 |
| C | 卷发/波浪 |
| D | 黑长直 |
| E | 无特定 |
| X | 自定义 |

**1.2A-6 拍摄风格偏好** ⭐
| # | 选项 | 说明 |
|---|------|------|
| A | **韩系/韩妆** | 柔光、暖调、奶油肌 |
| B | **日系/清透** | 逆光、透明感、空气感 |
| C | **法式/慵懒** | 自然光、随性、复古 |
| D | **写真/影楼** | 专业灯光、精致后期感 |
| E | **街拍/潮流** | 动态、抓拍、时尚感 |
| F | **Vogue/时尚杂志** | 高对比、戏剧光、couture |
| G | **Cinema Film Still** | 电影剧照感、叙事性、电影感 |
| H | **Editorial** | 编辑室风格、艺术感、画廊级 |
| X | **自定义** | 用户描述 |

→ 💡 辅助：**「让 NotebookLM 根据脸型+气质推荐最优风格组合」**

**1.2A-7 拍摄角度**
| # | 选项 |
|---|------|
| A | 正脸/直视镜头 |
| B | 侧脸/3/4侧颜 |
| C | 背影/侧背影 |
| D | 特写/面部细节 |
| E | 不刻意/抓拍自然 |
| X | 自定义 |

---

### Step 1.2B — 产品/物品

**1.2B-a 物品类型**
| # | 选项 |
|---|------|
| A | 化妆品/护肤品 |
| B | 食品/饮品 |
| C | 电子产品/数码 |
| D | 服饰/配件/包 |
| E | 珠宝/腕表 |
| F | 家具/家居 |
| X | 自定义 |

**1.2B-b 展示风格**
| # | 选项 |
|---|------|
| A | 极简/留白 |
| B | 场景化/生活情境 |
| C | 杂志大片感 |
| D | 建筑感/雕塑感 |
| X | 自定义 |

---

### Step 1.2C — 场景/风景

**1.2C-a 地点类型**
| # | 选项 |
|---|------|
| A | 都市/建筑/天际线 |
| B | 自然/山海/沙漠 |
| C | 咖啡馆/书店/室内 |
| D | 艺术空间/画廊/剧院 |
| E | 无特定/纯色/抽象 |
| X | 自定义 |

**1.2C-b 时间/天气**
| # | 选项 |
|---|------|
| A | 白天/晴/蓝天 |
| B | 黄昏/日落/魔幻时刻 |
| C | 夜晚/星空/蓝调 |
| D | 阴天/柔光/情绪感 |
| E | 雨天/雾气/电影感 |
| X | 自定义 |

---

## Layer 2 — 氛围层

### Step 2.1 色调基温

| # | 选项 | 效果 |
|---|------|------|
| A | 暖色系 | 橙/黄/红，温暖亲和 |
| B | 冷色系 | 蓝/青/绿，冷静理性 |
| C | 中性/低饱和 | 柔和内敛 |
| D | 高对比/强饱和 | 视觉冲击 |
| E | 黑白/单色 | 经典永恒 |
| X | **自定义** | 用户描述 |

---

### Step 2.2 胶片模拟风格 ⭐ 核心

| # | 选项 | 风格特点 | 人像适配度 |
|---|------|---------|----------|
| A | **Classic Chrome（CC）** | 低饱和、柔和、新闻杂志感 | ★★★★ |
| B | **Eterna / Cinema** | 电影院感，大动态，扁平色彩 | ★★★★ |
| C | **Classic Negative** | 高饱和、柔和肤色、怀旧感 | ★★★★★ |
| D | **Provia** | 标准中性、色彩平衡 | ★★★ |
| E | **Velvia** | 高饱和、高反差 | ★★ |
| F | **哈苏（Hasselblad）** | 中画幅，自然色彩科学，极高细节 | ★★★★★ |
| G | **徕卡（Leica）** ⭐ | 德系写实，空气感，柔和过渡，**真实皮肤质感首选** | ★★★★★ |
| H | **理光 GR（Ricoh）** | 街拍感，快照美学，真实感 | ★★★★ |
| I | **理光黑白（Ricoh B&W）** | 高对比黑白，街头纪实 | ★★★ |
| X | **自定义** | 用户描述想要的风格 |

→ 💡 辅助：**「让 NotebookLM 根据拍摄风格推荐最优胶片模拟组合」**

---

### Step 2.3 光线氛围

| # | 选项 | Prompt 关键词 |
|---|------|------------|
| A | 黄金时刻 | `golden hour lighting, warm sunlight, sun flare` |
| B | 柔光/阴天 | `overcast soft light, even lighting, diffused` |
| C | 窗光 | `natural window light, soft diffused light, side light` |
| D | 逆光/轮廓光 | `backlit, rim light, silhouette, sun behind subject` |
| E | 影棚灯光 | `studio lighting, professional flash, rim light` |
| F | 夜景/霓虹 | `neon light, cyberpunk lighting, blue hour` |
| G | 月光/自然光 | `moonlight, cool tone, ethereal, soft blue` |
| X | **自定义** | 用户描述 |

→ 💡 辅助：**「让 NotebookLM 根据场景推荐最优光线组合」**

---

### Step 2.4 真实感/质感 ⭐ 皮肤

> **女生人像核心**：皮肤质感是区别"AI感"和"真人感"的关键。
> **推荐组合**：Leica + 自然窗光 + Step 2.4-C/D + f/2.0~2.8

| # | 选项 | 关键词 | 说明 |
|---|------|-------|------|
| A | 追求完美 | `commercial grade, 8K, hyperrealistic` | 可能出现塑料感 |
| B | 真实随拍感 | `candid, unposed, slight motion blur, authentic moment` | 最自然 |
| C | **真实皮肤质感** | `realistic skin texture, detailed pores, natural skin tones, imperfections visible` | ★推荐，Leica 最配此风格 |
| D | 纪实胶片感 | `35mm film, documentary, authentic, film grain, raw` | 胶片颗粒+真实皮肤 |
| E | 精致奶油肌 | `glossy skin, bokeh, soft focus, editorial retouching` | 商业广告感，可能偏假 |
| X | **自定义** | 用户描述 | |

→ **选 C/D 时，自动推荐 Leica + 85mm + f/2.0~2.8**

---

### Step 2.5 镜头焦段 ⭐

| # | 选项 | 效果 | 说明 |
|---|------|------|------|
| A | **24mm 超广角** | 纳入环境，夸张透视 | 全身/场景感 |
| B | **35mm 小广角** | 兼顾环境与人像，纪实感 | 街拍/纪实人像 |
| C | **50mm 标准** | 接近人眼视角，自然无畸变 | 万能/室内 |
| D | **85mm 人像王** | 背景虚化，空间压缩，主体突出 | ★女生人像首选 |
| E | **135mm 中长焦** | 强虚化，突出人物，空气感 | 特写/氛围感 |
| F | **70-200mm 变焦** | 柔美虚化，构图灵活 | 活动/抓拍 |
| G | **中画幅数字** | Hasselblad look，极高细节 | 质感首选 |
| X | **自定义** | 用户描述 |

→ 默认推荐：**85mm**（女生人像黄金焦段）

---

### Step 2.6 光圈/虚化程度

| # | 选项 | 效果 |
|---|------|------|
| A | **f/1.2 - f/1.8** | 极浅景深，梦幻虚化 |
| B | **f/2.0 - f/2.8** | 柔美虚化，平衡感 |
| C | **f/4.0 - f/5.6** | 中等虚化，保留环境 |
| D | **f/8.0+** | 整体清晰，纪实风格 |
| X | **自定义** | 用户描述 |

---

### Step 2.7 机位/视角

| # | 选项 |
|---|------|
| A | 齐眼平视（自然视角）|
| B | 低角度仰拍（气场/显腿长）|
| C | 高角度俯拍（显小脸/可爱）|
| D | 特写/面部细节 |
| E | 远景/全身入镜 |
| X | 自定义 |

---

## Layer 3 — 技术层

> 纯输出规格，不影响画面风格。

### Step 3.1 尺寸比例

| # | 选项 | 比例 | 像素 |
|---|------|------|------|
| A | 小红书/Instagram | `3:4` | 864×1152 |
| B | X/Twitter 配图 | `16:9` | 1280×720 |
| C | 抖音/快手竖屏 | `9:16` | 720×1280 |
| D | 头像/方图 | `1:1` | 1024×1024 |
| E | 视频封面/横幅 | `21:9` | 1344×576 |
| F | 艺术装饰/巨幅 | `2:3` | 832×1248 |
| X | **自定义** | 用户指定尺寸 |

---

### Step 3.2 生成数量

| # | 选项 |
|---|------|
| A | 1 张 |
| B | 2 张（对比选择）|
| C | 3 张（Variations）|
| D | 4-9 张（批量）|
| X | 自定义数量 |

---

### Step 3.3 角色一致性

| # | 选项 |
|---|------|
| A | 否，直接生成 |
| B | 是，提供参考图 URL（`subject_reference`）|

---

### Step 3.4 复现需求

| # | 选项 |
|---|------|
| A | 不需要 |
| B | 需要（记录 seed，下次可复现）|

---

## NotebookLM 辅助选项

在 **Step 2.2（胶片模拟）**、**Step 2.3（光线）**、**Step 1.2A-6（人像风格）** 后可追加：

> 💡 **「让 NotebookLM 根据你的描述推荐最优组合」**
> → 将当前已收集的叙事层信息整理成描述
> → 调用 NotebookLM 查询推荐
> → 用户确认或修改

---

## Step 4 — 汇总输出（必须确认后才能生成）

组装完整 Prompt：

```
[主体描述] + [姿态/角度] + [环境/场景] + [胶片模拟] + [光线] + [色调] + [质感/真实感] + [镜头焦段] + [光圈] + [机位]
```

**必须执行：**
1. 显示完整 Prompt 给用户
2. 等用户确认
3. 用户确认后才能调用 API

**禁止：用户未确认就调用 API**

---

## 快速决策分支

| 用户说 | 默认组合 |
|--------|---------|
| 「随便生成一张」 | 暖色 + Classic Chrome + 黄金时刻 + 85mm f/2.8 |
| 「想要复古感」 | Classic Negative + 胶片颗粒 + 暖色调 + 135mm |
| 「想要电影感」 | Eterna Cinema + 宽幅 21:9 + 戏剧光 + 电影镜头 |
| 「要真实不要假」 | Classic Chrome + 35mm + 真实随拍感 |
| 「要有质感/高级感」 | 哈苏 + f/2.8 + 影棚光 + 精致质感 |
| 「要有艺术感/Gallery 级」 | Editorial + 中画幅 + 黑白或低饱和 |

---

## Prompt 关键词库

### 胶片模拟

| 模式 | 关键词 |
|------|-------|
| Classic Chrome | `Classic Chrome, film photography, muted tones, soft contrast, vintage magazine print` |
| Eterna Cinema | `Eterna cinema, 16mm film, cinematic color grading, flat dynamic, film LUT` |
| Classic Negative | `Classic Negative, nostalgic color, warm shadows, lifted blacks, retro film aesthetic` |
| 哈苏 | `Hasselblad medium format, natural color science, extremely high detail, clean tones` |
| 徕卡 | `Leica photography, German realism, subtle tonal transitions, airiness, filmic, natural skin texture, realistic skin tones, detailed pores, soft natural look` |
| 理光 GR | `Ricoh GR snapshot aesthetic, street photography, candid, natural` |
| 理光黑白 | `Ricoh GR B&W, high contrast black and white, street documentary` |

### 光线

| 光线 | 关键词 |
|------|-------|
| 黄金时刻 | `golden hour, warm sunlight, sun flare, backlit glow` |
| 窗光 | `natural window light, soft diffused light, translucent curtain light` |
| 逆光 | `backlit, rim light effect, silhouette, hair light` |
| 影棚 | `studio lighting, professional flash, beauty dish, rim light` |
| 霓虹 | `neon light, cyberpunk, blue hour, cinematic neon glow` |
| 月光 | `moonlight, cool ethereal, soft blue tones, dreamlike` |

### 镜头

| 焦段 | 关键词 |
|------|-------|
| 85mm | `85mm portrait lens, bokeh, subject isolation, compression` |
| 135mm | `135mm, telephoto, beautiful compression, creamy bokeh` |
| 35mm | `35mm wide angle, environmental portrait, documentary feel` |
| 中画幅 | `medium format digital, Hasselblad look, shallow depth of field` |
| 电影镜头 | `anamorphic lens, cinematic, film look, oval bokeh` |

---

## API 调用

```bash
# 基础（prompt 必须放最后）
minimax-generate -r 3:4 -o output.png "组装好的Prompt"

# 多张
minimax-generate -r 3:4 -n 3 -o output.png "Prompt"

# 角色一致性
minimax-generate -r 3:4 --ref character:URL -o output.png "Prompt"

# 固定种子（复现）
minimax-generate -r 3:4 --seed 42 -o output.png "Prompt"

# 完整参数
minimax-generate -r 3:4 -n 3 --ref character:URL --seed 42 -o output.png "Prompt"
```

---

## NotebookLM 研究笔记

**笔记本**: https://notebooklm.google.com/notebook/ce6db31e-ab63-47a1-bd41-8a30cba70b70

**理论依据**：Nano Banana 六要素（叙事控制）+ 富士色彩科学（氛围控制）+ GPT-4o 真实感技巧 + MiniMax API 完整参数

---

## 文件结构

```
minimax-image/
├── SKILL.md           # 本文件（引导流程）
├── _meta.json         # Skill 元数据
└── scripts/
    └── generate.sh    # API 调用脚本
```
