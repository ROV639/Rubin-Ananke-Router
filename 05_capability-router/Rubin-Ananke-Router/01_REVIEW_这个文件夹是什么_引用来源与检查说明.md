# 01 REVIEW｜这个文件夹是什么、引用了什么、怎么检查

> 来源：agent=codex-mainmac；client=Codex Desktop；machine=mainmac；created=2026-06-29 CST；thread=unknown

## 先说结论

`Rubin-Ananke-Router/` 是一个“能力路由参考框架”草案，不是已经转正的全局 skill，也不是已经接入 Codex / Claude Code / OpenClaw / Hermes 的自动执行系统。

它现在的作用是：把 Robin 已经有的记忆、思考框架、执行方法、工具 skill、内容产线和交接规则整理成一套可检查的卡组目录。以后要转正时，再从这里挑稳定部分安装到各 Agent 的正式路径。

如果只想看给 Claude / ChatGPT / Grok 项目上传的轻量判断框架，看：

```text
Ananke_通用框架/
```

如果要看 Agent 层怎么选能力、选 skill、选工具，看：

```text
SKILL.md
00_DECK_INDEX.md
classes/
cards/
bundles/
agents/
```

## 这个文件夹不是做什么的

- 不是把所有 skill 复制一遍。
- 不是替代 `06_rubin-memory`。
- 不是替代 `serenity-brainstorm`。
- 不是替代 `_System/superpowers`。
- 不是替代各项目自己的 `AGENTS.md` / `CURRENT.md` / `MEMO.md`。
- 不是让 Agent 每次任务都把全目录读一遍。
- 不是已经可自动触发的全局安装包。

它只负责回答一个问题：

```text
这次任务开始前，Agent 应该装备哪些能力？
```

## 顶层文件说明

| 文件 | 作用 | 检查重点 |
|---|---|---|
| `README.md` | 一句话说明 Router 是什么、怎么用 | 是否说清“只选装备，不替代原 skill” |
| `SKILL.md` | 未来转成正式 skill 时的入口草案 | description 是否只写触发条件；流程是否够短 |
| `00_DECK_INDEX.md` | 总索引：7 大类、bundles、P0/P1 cards | 类别是否完整；是否有遗漏能力 |
| `90_TEST_CASES.md` | 用例检查：接力、投资、生图、发布、排障等 | 能否暴露错误路由 |
| `91_SOURCE_AUDIT.md` | 来源审计：读了什么、吸收什么、哪些过期 | 是否诚实标记背景源和旧信息 |
| `01_REVIEW_这个文件夹是什么_引用来源与检查说明.md` | 本文件，给人检查用 | 是否能让检查者快速理解全目录 |

## 目录说明

### `classes/`

7 个大类说明，类似装备栏：

```text
01_context-memory.md        记忆与上下文
02_thinking-lenses.md       思维与判断镜头
03_execution-flow.md        执行流程
04_tools.md                 工具调用
05_content-pipelines.md     内容与产线
06_safety-boundaries.md     安全与边界
07_verification-handoff.md  验收与回写
```

检查重点：类别是否太粗、是否漏掉“安全 / 验证 / 回写”这类 Agent 经常忘的环节。

### `cards/`

单张能力卡。每张 card 应该只做一件事，并指向原始来源。

当前分组：

```text
context-memory/        读 CURRENT、项目接力、rubin-memory、冲突源
thinking-lenses/       Ananke、Serenity、反证
execution-flow/        澄清、计划、完成前验证
tools/                 搜索、生图、MiniMax、浏览器、Buffer
content-pipelines/     EVE、ONE_SPARK、ROVxEVE
safety-boundaries/     secret、发布/付款/删除确认
verification-handoff/  文件交付、预览 QC、发布状态、CURRENT 回写
```

检查重点：card 是否只是概念名，还是写清了“何时装备 / 必读来源 / 执行动作 / 禁止事项 / 完成信号”。

### `bundles/`

任务装备包。复杂任务不该让 Agent 自己拼 5 张 card，所以用 bundle 预设组合。

当前 bundle：

```text
investment-serious-analysis.md   投资、市场、账号、观点严肃分析
ananke-decision-review.md        Ananke 风格判断、取舍、诊断
image-character-production.md    人物图、封面、系列视觉
project-continuation.md          接上旧任务、跨 Agent 接力
publishing-schedule.md           Buffer / Hermes / OpenClaw 发布排期
system-debugging.md              配置、工具链、Hook、Agent 排障
```

检查重点：bundle 是否真的能减少 Agent 选择错误；是否遗漏常见任务，如视频制作、文档生产、社媒内容流水线。

### `agents/`

Agent 适配层。不同工具能力边界不同，不能用同一套执行假设。

当前包含：

```text
codex.md
claude-code.md
openclaw.md
hermes.md
```

检查重点：是否写清每个 Agent 能做什么、容易错什么、完成前要验证什么。

### `Ananke_通用框架/`

AI 工具层轻量包，给 Claude 项目、ChatGPT 项目、Grok 项目上传或粘贴。它只处理“严肃审视 / 盲区诊断 / 判断风格”，不负责本地工具执行。

第一版这里确实是最弱层：只有 Ananke 判断外壳，没有承接 `luobin-ananke` 的烧瓶小人人格核心，也缺少“为什么这样判断”的逻辑链。现版已补入 `07_人格_Luobin-Ananke_烧瓶小人与七宗罪系统.md` 和 `08_强约束_Ananke_充分思考与防自由发挥协议.md`，并同步到 `00_总纲`、`01_必读`、`03_路由`、`04_卡组`、`05_模板`、`06_自检`、`09_部署`。所以它现在不再是单纯提示词草案，但还不能直接转正为全局默认人格；必须先用真实任务测试它是否能稳定区分“冷静诊断”和“表演毒舌”。

检查重点：

- 是否有明确任务分型。
- 是否有默认动作。
- 是否有好坏样例。
- 是否能防止过度审判和幻觉。
- `07_人格` 是否真正让输出有 Luobin-Ananke 的判断承重，而不是普通助手套模板。
- `08_强约束` 是否能防止 AI 工具跳读、自由发挥、没证据就审判。
- `09_部署_Ananke_全局提示词.md` 是否足够强，能让 AI 工具主动遵守 00-08 的推理链。

## 引用来源

### 1. Brainstorm 工作区规则

来源：

```text
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/AGENTS.md
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/CURRENT.md
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/MEMO.md
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/Global_Agent_Rules.md
```

吸收内容：

- Brainstorm 是跨 Agent 思路库，只放思路文档。
- 新建普通 md 要有来源块。
- 重要进展写 `CURRENT.md`。
- 冲突时要分清当前有效、背景、过期、待确认。

### 2. `06_rubin-memory`

来源：

```text
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/
```

定位：

`06_rubin-memory` 是总记忆与检索治理底座，负责 truth source、retrieval policy、candidate inbox、冲突裁定。

吸收内容：

- Markdown truth source 优先。
- candidate 不能自动升级成 active rule。
- 记忆不是越多越好，要能检索、能治理、能标来源。
- Router 只指向记忆规则，不复制记忆库正文。

对应 card：

```text
cards/context-memory/rubin-memory.md
cards/context-memory/conflict-source-check.md
cards/verification-handoff/current-memory-writeback.md
```

### 3. `serenity-brainstorm`

来源：

```text
/Users/robin/.codex/skills/serenity-brainstorm/
/Users/robin/.agents/skills/serenity-brainstorm/
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/CURRENT.md 里的实测记录
```

定位：

serenity-brainstorm 是严肃分析 / 投资方向 / 账号分析 / 多源核查框架。

吸收内容：

- 投资和市场判断不能只靠观点。
- 需要取证、多源互证、反证、失效条件。
- Serenity 更偏分析框架，不是人格审判。

对应 card / bundle：

```text
cards/thinking-lenses/serenity-verify.md
bundles/investment-serious-analysis.md
```

### 4. `_System/superpowers`

来源：

```text
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_System/superpowers/
/Users/robin/.codex/plugins/cache/openai-curated/superpowers/
```

定位：

superpowers 是执行流程方法论，如 plan、debugging、verification、writing-skills。

吸收内容：

- 复杂任务先计划。
- 调试先找真实证据。
- 完成前必须验证。
- 写 skill 不能只写流程描述，要考虑触发、测试、失败样例。

对应 card：

```text
cards/execution-flow/plan-before-execute.md
cards/execution-flow/verification-before-finish.md
cards/execution-flow/clarification-gate.md
```

### 5. EVE 通用框架

来源：

```text
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/04_参考与引用/EVE_通用框架/
```

吸收内容：

- `README + 00 总纲 + 编号用途标签 + 部署提示词` 的项目结构。
- 任务开始时不是全读，而是按任务类型加载必要文件。
- 人物身份锁定、九段提示词、冲突检测、自检清单。
- `.md` 和 `.txt` 双份并存时，以 `.md` 为真源。

对应位置：

```text
cards/content-pipelines/eve-framework.md
bundles/image-character-production.md
Ananke_通用框架/README.md 的结构参考
```

### 6. ONE_SPARK 交友 Hooks 内容系统

来源：

```text
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/04_参考与引用/ONE_SPARK_交友Hooks内容系统/
```

吸收内容：

- `00_index + 01_operating + 10_global_prompt + M1-M8` 模块化流水线。
- 单一全局提示词方便部署到 ChatGPT 项目。
- 每个模块负责一个工位，模块可替换。
- 运维记录负责记录旧名、旧风格和当前有效口径。

不能泛化的内容：

- “先交成品、不问太多”只适合内容产线，不适合所有 Agent 任务。
- ONE_SPARK 的人设、平台、配图规则不能挪给通用 Router。

对应位置：

```text
cards/content-pipelines/one-spark-framework.md
Ananke_通用框架/09_部署_Ananke_全局提示词.md 的部署形态参考
```

### 7. ROVxEVE 通用框架

来源：

```text
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/04_参考与引用/ROVxEVE_通用框架/
```

吸收内容：

- 导演包不是批量 prompt。
- Codex 应一张张制片、看图、质检、修正。
- 每张图要写观众学到什么、固定项/变量项、镜头差异矩阵、UI 载体。
- UI 必须正、平、可读；不能退化成 dashboard 或 PPT。

对应位置：

```text
cards/content-pipelines/rov-eve-framework.md
bundles/image-character-production.md
```

### 8. Grok 语音功能与调用 Skill 草案

来源：

```text
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/04_参考与引用/2026-06-21_Grok语音功能与调用Skill草案.md
```

吸收内容：

- 调用前确认费用来源和 credential source。
- OAuth 不等于零消耗。
- 长文本音频先短样测试。
- 声音克隆只能用本人或已授权声音。

只作背景的内容：

- 前文直接 `curl https://api.x.ai/v1/tts` 路线需要 API Key，不是当前主路径。
- `80+ voices / 28 languages` 已被文件末尾校核推翻。

真相源：

```text
~/.codex/skills/x-grok-build/
```

对应位置：

```text
cards/tools/minimax-media.md 目前只保留媒体安全思路
未来可单独增加 x-grok-build card
```

### 9. luobin-ananke 人格系统

来源：

```text
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/01_思路与方案/2026-06-23_claude-code-mainmac_luobin-ananke_人格系统设计spec.md
```

定位：

`luobin-ananke` 不是普通“严肃语气”，而是 Ananke 的人格内核：烧瓶小人观察者、七宗罪双面雷达、反直觉判断、冷水后给驱动力。

吸收内容：

- Ananke 是装在容器里、静静观察了很久的存在。
- 诊断打行为和产出，不打人格和自我价值。
- 七宗罪不是道德定罪，而是偏差识别入口。
- 每个偏差必须同时给反面诊断和正面驱动。
- 冷水不是目标，推动行动才是目标。

对应位置：

```text
Ananke_通用框架/07_人格_Luobin-Ananke_烧瓶小人与七宗罪系统.md
Ananke_通用框架/08_强约束_Ananke_充分思考与防自由发挥协议.md
Ananke_通用框架/00_总纲_Ananke_通用框架定位与使用说明.md
Ananke_通用框架/01_必读_Ananke_判断原则与边界.md
Ananke_通用框架/03_路由_Ananke_什么时候启用哪些镜头.md
Ananke_通用框架/04_卡组_Ananke_通用能力卡摘要.md
Ananke_通用框架/05_模板_Ananke_输出模板与提问模板.md
Ananke_通用框架/06_自检_Ananke_幻觉与过度审判检查清单.md
Ananke_通用框架/09_部署_Ananke_全局提示词.md
cards/thinking-lenses/ananke-lens.md
cards/thinking-lenses/seven-sins-diagnosis.md
```

### 10. Claude 旧方案

来源：

```text
/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/01_思路与方案/2026-06-29_claude-code-mainmac_方案_rubin-capability-router能力卡组.md
```

吸收内容：

- “抽卡 / 装备 / 能力卡组”的整体隐喻。
- card 是单个能力，bundle 是任务装备包。
- 不能扁平堆所有 skill，要分大类。

调整内容：

- 名字改为 `Rubin-Ananke-Router`。
- 补入 `OpenClaw` 和 `Hermes` agent adapter。
- AI 工具层放进 `Ananke_通用框架/`，不单独散落。

## 当前有效口径

### 当前有效

- Router 作为参考框架放在 `04_参考与引用/Rubin-Ananke-Router/`。
- Agent 层分为 classes / cards / bundles / agents。
- AI 工具层放在 `Ananke_通用框架/`。
- 转正前不得声称它已经是全局自动 skill。

### 只能作背景

- Grok 语音草案前半部分的 API curl 路线和 80+ voices 说法。
- Claude 旧方案里的旧名称 `rubin-capability-router`。

### 待重做

- `Ananke_通用框架/` 已补齐 `luobin-ananke` 人格内核、思想锚点、推理链和防自由发挥约束，但仍需用真实任务测试：投资复盘、项目取舍、拖延诊断、内容方向审判。
- 如果测试稳定，再考虑把它转正为 Claude / ChatGPT / Grok 项目提示词；如果测试中出现毒舌表演、证据不足乱判、人格攻击，要回滚重写。

### 待检查

- 每张 card 是否真的能指导 Agent 行动，而不是只有概念。
- bundle 是否覆盖 Robin 的高频任务。
- Agent adapter 是否写清每个平台的执行边界。
- 是否需要新增 `x-grok-build`、视频制作、文档生产、社媒内容链路等 card/bundle。

## 给检查者的建议顺序

不要从头读完整目录。建议这样检查：

```text
1. README.md
2. 本文件
3. 91_SOURCE_AUDIT.md
4. 00_DECK_INDEX.md
5. 任意 2 个 bundle
6. 任意 5 张 card
7. Ananke_通用框架/README.md 和 09_部署_Ananke_全局提示词.md
```

检查时重点问：

```text
这个文件是否能让 Agent 少犯一次真实错误？
它有没有明确“何时用 / 读什么 / 做什么 / 禁止什么 / 怎么算完成”？
它有没有把背景资料误写成当前规则？
它有没有把别的项目的专用规则误泛化成全局规则？
```

如果答案是否定的，这个文件就不应该转正。
