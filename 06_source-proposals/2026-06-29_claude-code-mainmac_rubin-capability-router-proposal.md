# Rubin Capability Router 能力卡组方案

> 来源：agent=claude-code-mainmac；client=claude-code-vscode；machine=mainmac；created=2026-06-29 01:19 CST；thread=unknown

## 0. 结论

建议不要把 Ananke 单独扩成一个“超级通用审视框架”。

更稳的路线是做一个上层系统：

```text
rubin-capability-router
= Robin 全 Agent 能力卡组与装备系统
```

Ananke、Serenity、Superpowers、搜索、生图、发布、记忆、验收，都作为它下面的 card / bundle 被调度。

一句话：

> 不要让 Ananke 负责一切。让 Router 决定什么时候装备 Ananke。

## 1. 为什么不用“单独 Ananke 大框架”

单独做 `ANANKE_通用审视框架` 的优点是快、聚焦、适合 Claude / ChatGPT / Grok 复制。

但它只解决“怎么思考 / 怎么问 / 怎么审视”，不解决更关键的问题：

- 什么时候该用 Ananke？
- 什么时候该用 Serenity 取证？
- 什么时候该先读 CURRENT / MEMO？
- 什么时候必须联网？
- 什么时候必须验收？
- 什么时候必须回写？
- Hermes / Codex / Claude / ChatGPT / Grok 的能力差异怎么处理？

如果只做 Ananke，后面仍会变成一堆独立 skill / prompt，Agent 还是会漏调用。

所以 Ananke 应该降级为一组 thinking-lenses 卡，而不是总系统。

## 2. 推荐总架构

建议在：

```text
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/04_参考与引用/rubin-capability-router/
```

建立以下结构：

```text
rubin-capability-router/
├── README.md
├── SKILL.md
├── 00_DECK_INDEX.md
├── classes/
│   ├── 01_context-memory.md
│   ├── 02_thinking-lenses.md
│   ├── 03_execution-flow.md
│   ├── 04_tools.md
│   ├── 05_content-pipelines.md
│   ├── 06_safety-boundaries.md
│   └── 07_verification-handoff.md
├── cards/
│   ├── context-memory/
│   ├── thinking-lenses/
│   ├── execution-flow/
│   ├── tools/
│   ├── content-pipelines/
│   ├── safety-boundaries/
│   └── verification-handoff/
├── bundles/
│   ├── investment-serious-analysis.md
│   ├── ananke-decision-review.md
│   ├── image-character-production.md
│   ├── project-continuation.md
│   ├── publishing-schedule.md
│   └── system-debugging.md
├── adapters/
│   ├── claude-code.md
│   ├── codex.md
│   ├── hermes.md
│   └── chatgpt-grok.md
└── 90_TEST_CASES.md
```

## 3. 核心概念

### 3.1 Card

Card 是单个能力。

例：

```text
ananke-lens
serenity-verify
clarification-gate
chatgpt-imagegen
verification-before-finish
secret-redaction
current-memory-writeback
```

每张 card 应该短，不写成长篇手册。

建议固定格式：

```text
# card-name

## 何时装备
## 解决什么问题
## 必读来源
## 执行动作
## 禁止事项
## 完成信号
```

### 3.2 Bundle

Bundle 是一套任务装备。

例：投资严肃分析不是一张卡，而是一组卡：

```text
investment-serious-analysis
= brainstorm-current
+ rubin-memory
+ search-router
+ serenity-verify
+ counterargument-check
+ ananke-lens
+ verification-before-finish
```

Bundle 固定格式：

```text
# bundle-name

## 适用任务
## 装备卡组
## 执行顺序
## 必须验证
## 回写要求
```

### 3.3 Adapter

Adapter 解决跨 Agent 差异。

同一张 card 不应该给 Claude / Codex / Hermes / ChatGPT / Grok 写多份。

差异放 adapter：

- Claude Code：本地读文件、可用 Claude skills、适合方案治理和代码任务。
- Codex：生产主端，适合执行、多文件、媒体链、脚本、Remotion、批处理。
- Hermes：发布 / 自动化端，必须加强“是否真的完成 / 是否截图 / 是否排期”。
- ChatGPT / Grok：不能依赖本地路径，靠上传知识库或粘贴总提示词。

## 4. 七个能力大类

保留 Robin 提出的 7 类，这个分类是对的。

### 4.1 记忆与上下文

目标：先知道现在聊到哪，避免重复、断线、误判。

核心来源：

- `06_rubin-memory`
- `AGENTS.md / CLAUDE.md`
- `CURRENT.md`
- `MEMO.md`
- 项目接力文档
- 冲突源 / 旧决定 / 候选记忆

第一版 cards：

```text
brainstorm-current
brainstorm-memo
project-handoff
rubin-memory
conflict-source-check
```

### 4.2 思维与判断镜头

目标：给任务装备不同的判断镜头，而不是默认普通助手。

核心来源：

- `serenity-brainstorm`
- `luobin-ananke`
- 投资分析链路
- 七宗罪盲区诊断
- 反例 / 反方检查

第一版 cards：

```text
ananke-lens
seven-sins-diagnosis
serenity-verify
investment-thesis-check
counterargument-check
```

### 4.3 执行流程

目标：让 Agent 不跳步，不抢答，不假完成。

核心来源：

- `_System/superpowers`
- planning
- debugging
- verification
- code review
- finishing branch

第一版 cards：

```text
clarification-gate
plan-before-execute
debug-loop
verification-before-finish
finish-branch
```

### 4.4 工具调用

目标：让 Agent 知道应该调用哪个工具，而不是乱造脚本 / 错用搜索。

第一版 cards：

```text
search-router
chatgpt-imagegen
minimax-media
browser-control
buffer-publish
```

工具映射：

- 搜索：`api-search-media-route` / Tavily / multi-search
- 生图：`image_gen` / `chatgpt-imagegen` / `gpt-image-2`
- MiniMax：TTS / 音乐 / STT / 视频理解
- 浏览器：browser / chrome / computer use
- 发布：Hermes / OpenClaw / Buffer

### 4.5 内容与产线

目标：把内容生产线做成可装备能力，而不是临时想起。

第一版 cards：

```text
rml-video
rov-eve-image
paper-moon-video
arthur-opinion
one-spark-social
```

对应产线：

- RML 视频
- Rov / Eve 图像
- Paper Moon 音乐 / 视频
- Arthur 观点视频
- ONE SPARK / 社媒矩阵

### 4.6 安全与边界

目标：防止 Agent 自动做不可逆动作。

第一版 cards：

```text
secret-redaction
delete-overwrite-confirm
publish-confirm
payment-remote-confirm
```

覆盖：

- key / token / cookie
- 删除 / 覆盖
- 发布 / 排期
- 付款
- 远程操作
- 敏感信息脱敏

### 4.7 验收与回写

目标：任务不是“说完”，而是“交付、验证、接力”。

第一版 cards：

```text
file-delivered-check
preview-qc-check
publish-status-check
current-memory-writeback
```

覆盖：

- 是否真的生成文件
- 是否真的发布 / 排期
- 是否截图 / 预览 / QC
- 是否更新 `CURRENT.md`
- 是否写入 memory inbox

## 5. 第一版 bundles

第一版只做 6 个 bundle，覆盖 80% 高频任务。

### 5.1 investment-serious-analysis

适用：投资、市场、项目、账号、观点严肃分析。

装备：

```text
brainstorm-current
rubin-memory
search-router
serenity-verify
investment-thesis-check
counterargument-check
ananke-lens
verification-before-finish
```

特点：

- Serenity 负责事实链和取证。
- Ananke 负责盲区和严肃诊断。
- Counterargument 负责反证。
- Verification 负责确认不是单一来源幻觉。

### 5.2 ananke-decision-review

适用：Robin 要审视一个计划、取舍、拖延、分散、执行困境。

装备：

```text
clarification-gate
ananke-lens
seven-sins-diagnosis
counterargument-check
verification-before-finish
```

特点：

- 信息不足先问。
- 信息充足才判决。
- 不编 Robin 历史。
- 输出必须有下一步动作。

### 5.3 image-character-production

适用：EVE / ROV / ONE SPARK / 人物图像 / 封面 / 视觉系列。

装备：

```text
project-handoff
chatgpt-imagegen
rov-eve-image
preview-qc-check
file-delivered-check
```

特点：

- 先读项目框架。
- 不直接大批量生成。
- 预览 / QC 必须落地。
- 不反复灌高清图进上下文。

### 5.4 project-continuation

适用：接上旧线程、跨 Agent 接力、找 session、继续之前任务。

装备：

```text
brainstorm-current
brainstorm-memo
project-handoff
conflict-source-check
current-memory-writeback
```

特点：

- 先读 CURRENT / MEMO。
- 查目标项目接力。
- 找冲突源。
- 完成后回写。

### 5.5 publishing-schedule

适用：Buffer / Hermes / OpenClaw 发布、排期、社媒同步。

装备：

```text
buffer-publish
publish-confirm
publish-status-check
current-memory-writeback
```

特点：

- 发布前必须确认账号、正文、媒体、时间。
- 发布后必须查状态。
- Hermes 端必须截图或返回状态凭证。

### 5.6 system-debugging

适用：配置、脚本、hook、工具链、代码错误、部署问题。

装备：

```text
plan-before-execute
debug-loop
verification-before-finish
secret-redaction
finish-branch
```

特点：

- 先复现 / 查日志。
- 改动后必须验证。
- 涉及 secret 必须脱敏。
- Git 分支 / 提交 / PR 按上下文决定。

## 6. Router 行动流程

Agent 收到任务后，按这个顺序：

```text
用户任务
→ 判断任务类别
→ 抽取 1-2 个大类
→ 选择 1 个 bundle 或 2-5 张 card
→ 读取对应原 skill / 原文
→ 执行
→ 验收
→ 回写接力 / 候选记忆
```

注意：

- 不要一次加载所有卡。
- 不要让 Router 变成百科。
- Router 只负责选装备。
- 具体执行逻辑仍回到原 skill / 原文。

## 7. SKILL.md 设计原则

`SKILL.md` 不能长。

只写：

```text
1. 识别任务类型
2. 选择 bundle / card
3. 读取对应文件
4. 执行后必须验收 / 回写
```

禁止把所有能力细节塞进 `SKILL.md`。

否则模型会只读总入口，不读卡片。

## 8. 与现有资料的关系

### 8.1 04_参考与引用

用它的格式：

- `README.md`
- `00_index.md`
- `01_operating.md`
- `10_global_prompt.md`
- `11_deploy_claude.md`
- `12_startup_prompts.md`

尤其参考：

- `ONE_SPARK_交友Hooks内容系统/00_index.md`
- `ONE_SPARK_交友Hooks内容系统/01_operating.md`
- `ONE_SPARK_交友Hooks内容系统/10_global_prompt.md`
- `EVE_通用框架/README.md`
- `ROVxEVE_通用框架/00_总纲_ROVxEVE_项目定位与使用说明.md`

### 8.2 serenity-brainstorm

抽成 thinking-lenses 和 bundles：

- `serenity-verify`
- `investment-thesis-check`
- `counterargument-check`
- `investment-serious-analysis`

要保留它的核心纪律：

- 一手材料优先
- 多源互证
- 单一来源不下结论
- 冲突源不抹平
- confidence / disclosure
- 主动找反证

### 8.3 _System/superpowers

抽成 execution-flow：

- `clarification-gate`
- `plan-before-execute`
- `verification-before-finish`
- `current-memory-writeback`

要保留它的核心：

- 先分类
- 再提问
- 再执行
- 验证后收口

### 8.4 luobin-ananke

抽成 thinking-lenses：

- `ananke-lens`
- `seven-sins-diagnosis`
- `ananke-decision-review`

要保留它的核心：

- 信息不足先问
- 有据才判决
- 七宗罪作为盲区镜头
- 泼冷水后必须给驱动力
- 不打人格，只打行为 / 产出层

## 9. ChatGPT / Grok 通用化

这个系统不能只给 Claude Code 用。

因此 `adapters/chatgpt-grok.md` 和 `10_global_prompt.md` 必须提供“无本地路径版本”。

ChatGPT / Grok 的使用方式：

```text
1. 上传 00_DECK_INDEX + 需要的 cards / bundles
2. 或直接粘贴 10_global_prompt
3. 让模型先选 bundle / card
4. 再让它执行
5. 要求输出“已装备卡组”
```

ChatGPT / Grok 不能假装读到了本地文件。

如果文件没上传，就必须说：

```text
我没有该卡片内容。请上传或粘贴。
```

## 10. 第一版验收测试

建议 `90_TEST_CASES.md` 至少包含：

### 10.1 Ananke 闸门测试

输入：

```text
我这三个项目怎么样？
```

预期：

```text
先问哪三个项目，不脑补。
```

### 10.2 投资分析测试

输入：

```text
帮我分析某只股票 / 某个加密项目是否值得关注。
```

预期：

```text
装备 investment-serious-analysis；先取证，不单一来源下结论。
```

### 10.3 生产任务测试

输入：

```text
帮我做一组 EVE 图。
```

预期：

```text
装备 image-character-production；先读项目框架；不直接批量生成。
```

### 10.4 发布任务测试

输入：

```text
帮我发 Buffer。
```

预期：

```text
装备 publishing-schedule；必须先确认账号、内容、媒体、时间。
```

### 10.5 接力任务测试

输入：

```text
继续上次任务。
```

预期：

```text
装备 project-continuation；先读 CURRENT / MEMO / 项目接力。
```

## 11. 最小执行版本

Codex 第一轮不要全做 100 张卡。

建议最小交付：

```text
1 个 README
1 个 00_DECK_INDEX
1 个 SKILL.md
7 个 class 文件
24-28 张核心 card
6 个 bundle
4 个 adapter
1 个测试文件
```

核心卡优先级：

```text
P0：clarification-gate / ananke-lens / serenity-verify / search-router / verification-before-finish / current-memory-writeback / secret-redaction
P1：brainstorm-current / project-handoff / counterargument-check / plan-before-execute / browser-control / publish-confirm
P2：内容产线与专用工具卡
```

## 12. 风险与反方论点

### 风险 1：Router 变成长文档，Agent 不读

解决：

- `SKILL.md` 极短。
- card 短。
- bundle 只列装备顺序。
- 细节回源文件。

### 风险 2：Card 复制原 skill，形成双写漂移

解决：

- card 只写“何时装备 / 必读来源 / 禁止事项”。
- 不复制原 skill 全文。
- 原 skill 是事实源，card 是路由层。

### 风险 3：ChatGPT / Grok 无法读本地路径

解决：

- adapter 单独写无本地路径方案。
- 让模型承认“未上传则不可用”。
- `10_global_prompt.md` 提供可粘贴版本。

### 风险 4：Hermes 执行后假完成

解决：

- Hermes adapter 强制完成凭证。
- 发布 / 排期 / 浏览器任务必须有状态返回或截图。

### 风险 5：Ananke 过强导致所有任务都像审判

解决：

- Ananke 只是 thinking-lens card。
- 只有 decision review / serious analysis 才装备。
- 普通生产任务不默认装备 Ananke。

## 13. 给 Codex 的执行提示词草案

```text
你要在 Brainstorm 的 04_参考与引用 下创建 rubin-capability-router 第一版。

参考方案：
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/01_思路与方案/2026-06-29_claude-code-mainmac_方案_rubin-capability-router能力卡组.md

目标：
做一个跨 Claude / Codex / Hermes / ChatGPT / Grok 的能力卡组与装备系统。

要求：
1. 不做超级长 skill。
2. SKILL.md 只负责任务分类、选择 bundle/card、读取对应文件。
3. card 只写何时装备、来源、动作、禁止事项、完成信号，不复制原 skill 全文。
4. bundle 是装备包，写适用任务、卡组、顺序、验收、回写。
5. adapter 处理不同 Agent 差异。
6. 第一版只做 20-30 张核心卡和 6 个 bundle。
7. 参考 04_参考与引用 现有框架格式，文件要有 README / index / 使用说明 / deploy prompt / test cases。
8. 新建 md 文件必须遵守 Brainstorm 命名和来源署名规则。

先完成文件结构和 P0/P1 核心卡，不要贪全。
```

## 14. 最终判断

这条路线可行，而且比“做一个超级总 skill”稳。

真正要解决的不是“Robin 有多少 skill”，而是：

```text
Agent 在具体任务下，知道该装备哪些能力。
```

Rubin Capability Router 就是这层装备系统。

Ananke 是刀。
Serenity 是放大镜。
Superpowers 是流程。
Memory 是地图。
Verification 是收口。
Router 是决定此刻该拿哪几件装备的人。
