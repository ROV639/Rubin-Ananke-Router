---
title: 2026-06-17 Consensus Audit
index_id: consensus-audit-2026-06-17
memory_layer: initial
status: active
scope:
  - content
  - publishing
  - ops
source_system:
  - brainstorm
  - rubin-memory
memory_type:
  - decision
  - consensus
sensitivity: low
time_sensitivity: current
canonical: false
last_reviewed: 2026-06-20
source_paths:
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/
exclude_from_startup: true
---

# 21_2026-06-17-consensus-audit

> 对 `02_决定与共识/` 中 2026-06-17 六份共识文件的有效性审阅。用途：决定哪些内容进入 `06_rubin-memory` 初始化记忆，哪些只作为历史参考或候选。

## 审阅范围

```text
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/2026-06-17_Buffer社媒发布接入说明_全Agent与双电脑.md
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/2026-06-17_共识_AI内容工作室_当前主任务_全Agent接入.md
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/2026-06-17_共识_Buffer国际线发布规划与API账号映射.md
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/2026-06-17_决定_社媒变现锚定方案_小红书AI视觉旗舰.md
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/2026-06-17_平台方法论_各社媒账号怎么做_全Agent接入.md
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/02_决定与共识/2026-06-17_指南方针_AI内容工作室_多平台布局与操作手册.md
```

---

# 1. 总体裁定

## 仍有效，应该进入初始化记忆

这些是当前仍应作为跨 Agent 共识的内容：

1. Robin 做的是 **AI 内容工作室**，不是 AI 频道套利。
2. 当前锚点是 **变现**，所有运营动作都要回答“离变现更近了吗”。
3. **小红书 = 主打变现阵地**，Garfield2025 是 AI 视觉实操中长视频/深度图文旗舰。
4. **抖音 = 基本盘 / 涨粉 / 钩子验证阵地**，ONCELUO2023 是 AI 判断基本盘。
5. 艺斌视觉线 + 泽川 AI 判断线两手抓；账号只有轻重，没有冷藏。
6. 护城河是生产系统 × 判断 / 审美 / 人格 / 自有受众 / IP，不是“会用 AI”。
7. 人类判断链不可外包：选题、观点、钩子、人设、审美终审、商业取舍、平台策略必须由 Robin / Claude / DeepSeek 这类判断链确认。
8. Agent 不要瞎编观点，不要把自动化流程当护城河。
9. Buffer 是 IG/X/Threads 国际线轻量分发层，不是主阵地，不抢小红书/抖音/公众号主产能。
10. 发布前必须确认账号、正文、媒体 URL、发布时间、是否立即发布；API key 不进文档、不进 Git、不进聊天。
11. 平台方法论中的小红书、抖音、公众号、快手、视频号、B站轻维护规则仍有参考价值，但平台机制需按日期复核。

## 已被后续文件覆盖或降级

这些内容不能作为当前 active 共识直接加载：

1. `2026-06-17_指南方针_AI内容工作室_多平台布局与操作手册.md` 里“B站 = 旗舰主场（确认）”与后续 v2 共识冲突。
2. 当前应以 `2026-06-17_共识_AI内容工作室_当前主任务_全Agent接入.md` v2 和 `2026-06-17_决定_社媒变现锚定方案_小红书AI视觉旗舰.md` 为上位共识。
3. B站当前不是主旗舰，而是“轻·维护观察 / 遇好思路好素材再加码 / 不作废”。
4. `指南方针` 仍可作为历史方案和操作素材库，但其中平台主次必须以 v2 共识修正。

## 需要定期复核

以下内容含平台动态，不能永久当事实：

- 小红书算法、创作基金、笔记付费、世界杯转播权等平台窗口。
- 抖音行为预测、完播/点赞机制、账号权重。
- 快手播放量水分判断。
- 公众号公域流量占比、AI 水文识别。
- Threads / X / IG 的发布节奏和账号策略。
- Buffer channel 状态、GraphQL 字段、图床链路。

---

# 2. 单文件审阅

## 2.1 Buffer 社媒发布接入说明

```yaml
status: active
memory_type: publishing/workflow/security
source_path: 02_决定与共识/2026-06-17_Buffer社媒发布接入说明_全Agent与双电脑.md
recommend_init: yes-selective
```

### 仍有效

- 三个 Buffer key 分组对应 9 个国际 channel。
- Key 在统一凭据库，只能脱敏引用，不输出值。
- Buffer 发布执行以 Hermes 为主，OpenClaw 已接入，Codex 知晓不抢发布。
- ChatGPT 不参与发布执行。
- 发布、删除、批量排期前逐项确认。
- 评论不靠 Buffer，Buffer 只做发布/排期。
- 配图发图链路：本地/生成图片 → 图床公开 URL → Buffer createImagePost。
- 旧电脑 Codex 要发 Buffer，必须 Robin 手动同步 key；读不到则只读不发。

### 进入初始化记忆

- 发布执行边界。
- Key 脱敏和凭据库规则。
- Buffer 不是评论/互动工具。
- Codex 生产、Hermes/OpenClaw 发布的边界。

### 不进入初始化记忆全文

- 局域网 IP。
- 具体 GraphQL 细节只进 workflow/Buffer 专项，不进 core。
- 具体 channel 列表可进 source/publishing 索引，但不进最短 core。

## 2.2 AI 内容工作室当前主任务 v2

```yaml
status: active-primary
memory_type: consensus/content/agent-boundary
source_path: 02_决定与共识/2026-06-17_共识_AI内容工作室_当前主任务_全Agent接入.md
recommend_init: yes
```

### 仍有效

这是 6 月 17 日社媒矩阵最重要的上位共识之一。

核心有效内容：

- AI 内容工作室，不是 AI 频道套利。
- 锚点 = 变现。
- 选题赛道 > 个人努力。
- 小红书主打变现，抖音主打基本盘/涨粉。
- 艺斌视觉线 + 泽川 AI 判断线两手抓，无账号冷藏。
- 变现旗舰赛道 = AI 视觉实操 + 判断的差异化中长视频/深度图文。
- 护城河 = 生产系统 × 判断，不是“会用 AI”。
- 人类判断链不可外包。
- ChatGPT/MiniMax 是工具，不是自主执行 Agent。
- 合规红线：AIGC 标注、不碰伪养生/医疗/低俗擦边、不荐股、不教翻墙、不硬写跨平台账号引流。

### 进入初始化记忆

应进入 `01_initial-memory.md` 和 `02_core-consensus.md` 的内容：

- 一句话定位。
- 锚点 = 变现。
- 护城河原则。
- 人类判断链不可外包。
- 工具 vs 自主执行 Agent 的边界。
- 合规红线摘要。

### 不直接进入 core 的内容

- 具体 P0/P1/P2 执行顺序，属于当前阶段计划，放 project/content memory。
- 账号轻重表可进 content memory，不必进最短 core。
- 平台窗口数据需要定期复核。

## 2.3 Buffer 国际线发布规划与 API 账号映射

```yaml
status: active
memory_type: publishing/tool/security
source_path: 02_决定与共识/2026-06-17_共识_Buffer国际线发布规划与API账号映射.md
recommend_init: yes-selective
```

### 仍有效

- Buffer 是国际线轻量分发层，不是主阵地。
- 核心用途是把已生产内容低成本同步到 IG/X/Threads。
- 不抢小红书/抖音/公众号主产能。
- 媒体必须先托管为公开、直连、HTTPS、稳定 URL。
- 不做全自动批量铺量，不为 X/Threads/IG 单独开重制作流程。
- 发布前必须确认账号、正文、媒体 URL、发布时间、是否立即发布。
- 金融内容不输出买卖、目标价、仓位或账户操作。
- API key 不进文档、不进 Git、不进聊天，只使用环境变量。

### 进入初始化记忆

- Buffer 轻量分发定位。
- 媒体 URL 合格标准。
- 发布安全检查。

### 只进 source/publishing，不进 core

- 具体 channel 映射。
- 推荐每日/每周节奏。
- 推荐内容池。

## 2.4 社媒变现锚定方案：小红书 AI 视觉旗舰

```yaml
status: active-primary
memory_type: decision/content/monetization
source_path: 02_决定与共识/2026-06-17_决定_社媒变现锚定方案_小红书AI视觉旗舰.md
recommend_init: yes
```

### 仍有效

这是“锚点决策”，优先级很高。

核心有效内容：

- 平台风口和 Robin 护城河在此刻重叠，因此选择小红书 AI 视觉变现旗舰。
- 主阵地 = 小红书，抖音 = 稳基本盘 + 钩子验证。
- 不做 AI 头像/AI 写真低门槛红海。
- 做“AI 视觉怎么做出来、为什么这么做”的深度实操中长视频。
- 护城河 = 生产系统 × 判断。
- 小红书、抖音都发力，只有轻重，没有冷藏。
- Garfield2025 = 变现旗舰；ONCELUO2023 = 判断基本盘。
- 变现阶梯：平台激励/创作基金 → 蒲公英商单 → 笔记付费/知识付费 → 接单/模板/私域课程。
- 合规：AIGC 标注、不碰伪养生/医疗、不割韭菜、不用“月入几万”话术。

### 进入初始化记忆

- “小红书 AI 视觉变现旗舰”这一主锚。
- “AI 视觉实操 + 判断”赛道定义。
- “不做低门槛 AI 头像/写真红海”。
- “轻重不等于冷藏”。

### 需复核

- 平台扶持事实是 2026-06-17 核验，后续执行前需查当天是否仍有效。

## 2.5 平台方法论：各社媒账号怎么做

```yaml
status: active-reference
memory_type: project/platform-methodology
source_path: 02_决定与共识/2026-06-17_平台方法论_各社媒账号怎么做_全Agent接入.md
recommend_init: partial
```

### 仍有效

作为各账号接手前的操作参考仍有效：

- Garfield2025：小红书重·变现旗舰。
- Once369：抖音中·视觉钩子。
- Roven639：快手轻·反馈池。
- Rubin639：小红书中·AI 判断图文。
- ONCELUO2023：抖音重·判断基本盘。
- 亚瑟的观测局：公众号中·信任沉淀。
- 视频号：轻·联动。
- 泽川639：B站轻·维护候选，不作废。
- 大己贵/懒觉天天睡：变现验证。

平台打法中仍有用：

- 小红书看收藏/关注/观看时长，不只看完播。
- 小红书标题埋搜索词，封面写结果，不诗性开头。
- 抖音前 3 秒兑现标题承诺。
- 快手播放接近 3000 要警惕保底水分。
- 公众号不日更冲量，重单篇质量和私域承接。
- B站不当旗舰，但保留观察和素材沉淀。
- 一鱼多吃：小红书中长视频/图文、抖音切片、公众号长文、视频号、快手同步。

### 进入初始化记忆

只进入账号速查和核心平台指标，不进入每个平台长篇机制。

### 需复核

平台算法与数据变化快，本文件应标注 `time_sensitive: true`。执行具体账号任务前需重查平台机制。

## 2.6 多平台布局与操作手册

```yaml
status: partially-superseded
memory_type: strategy/workflow/archive-reference
source_path: 02_决定与共识/2026-06-17_指南方针_AI内容工作室_多平台布局与操作手册.md
recommend_init: partial-no-core
```

### 仍有价值

这份文档有大量可复用方法论：

- 全球创作者经济转向自有变现。
- 护城河原则：判断不外包、观点领跑、复利来自自有资产。
- 一鱼多吃生产流水线。
- 生图队列协议、Remotion 收窄、包装门禁、情报输入流水线、音频字幕基础设施复用。
- CONTENT_MOAT.md 选题门禁。
- 合规红线手册。
- 私域/社群/咨询作为长线复利方向。
- EVE / ROV / Paper Moon 作为角色/IP 长线资产。

### 已过时或被覆盖

- “B站 = 旗舰主场（确认）”已被 v2 共识覆盖。
- 当前主锚不是 B站，而是小红书 AI 视觉变现旗舰 + 抖音判断基本盘。
- 本文件标题写“待 Robin 拍板后转正式执行版”，说明它不是最终单一真相源。

### 进入初始化记忆

可抽取：

- 判断不外包。
- 一鱼多吃流水线。
- 生图队列协议。
- 包装门禁。
- CONTENT_MOAT 选题门禁。
- 自有变现层作为长线方向。

不进入 core：

- B站旗舰主场。
- 具体阶段里程碑。
- 未拍板人格锚。

---

# 3. 应写入 `06_rubin-memory` 的新增记忆

## 3.1 新增到 `01_initial-memory.md`

建议补充：

```text
- 2026-06-17 的上位社媒共识以 “AI 内容工作室 v2” 和 “小红书 AI 视觉变现旗舰” 为准。
- “指南方针”里 B站旗舰表述已被 v2 覆盖，只保留方法论和 workflow 价值。
- 小红书 AI 视觉实操中长视频是当前变现旗舰；抖音 ONCELUO2023 是判断基本盘。
- 账号只有轻重，不冷藏。
- 内容灵魂保留在人类判断链；Agent 只做生产、检索、整理、执行，不现场编观点。
- Buffer 是国际线轻量分发层，发布需确认账号/正文/媒体URL/时间/是否立即发。
- 平台机制类记忆必须带核验日期，执行前按需重查。
```

## 3.2 新增到 `20_conflict-index.md`

建议补充：

```text
- 2026-06-17_指南方针 的 “B站 = 旗舰主场” 与后续 v2 共识冲突；当前以小红书旗舰为准。
- 2026-06-17 平台方法论是 time-sensitive active-reference，不是永久事实。
```

## 3.3 新增到 `30_retrieval-policy.md`

建议补充：

```text
查询社媒矩阵/平台打法时：
1. 先读 AI 内容工作室 v2 或其摘要。
2. 再读小红书 AI 视觉旗舰锚定方案。
3. 具体账号任务再读平台方法论对应段。
4. “指南方针”只读 workflow/方法论段，不读 B站主次结论。
```

---

# 4. 结论

这 6 份文件不是同等优先级：

1. **最高优先级 active-primary**：
   - `2026-06-17_共识_AI内容工作室_当前主任务_全Agent接入.md`
   - `2026-06-17_决定_社媒变现锚定方案_小红书AI视觉旗舰.md`

2. **active publishing reference**：
   - `2026-06-17_Buffer社媒发布接入说明_全Agent与双电脑.md`
   - `2026-06-17_共识_Buffer国际线发布规划与API账号映射.md`

3. **active time-sensitive reference**：
   - `2026-06-17_平台方法论_各社媒账号怎么做_全Agent接入.md`

4. **partially superseded workflow/reference**：
   - `2026-06-17_指南方针_AI内容工作室_多平台布局与操作手册.md`

最终给 `06_rubin-memory` 的处理原则：

- 抽取 v2 共识和小红书旗舰锚定为 active memory。
- 抽取 Buffer 发布边界为 publishing/security memory。
- 抽取平台方法论为 time-sensitive project memory。
- 抽取指南方针的方法论，不抽取其 B站旗舰结论。
