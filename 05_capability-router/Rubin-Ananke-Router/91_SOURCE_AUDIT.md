# 91 Source Audit｜04_参考与引用盘点结论

> 来源：agent=codex-mainmac；client=Codex Desktop；machine=mainmac；created=2026-06-29 CST；thread=unknown

## 已读范围

- `EVE_通用框架/`
- `ROVxEVE_通用框架/`
- `ONE_SPARK_交友Hooks内容系统/`
- `2026-06-21_Grok语音功能与调用Skill草案.md`
- `06_rubin-memory/`
- `01_思路与方案/2026-06-23_claude-code-mainmac_luobin-ananke_人格系统设计spec.md`
- `01_思路与方案/2026-06-29_claude-code-mainmac_方案_rubin-capability-router能力卡组.md`

## 复盘方式

- 先读 Brainstorm 的 `AGENTS.md` / `CURRENT.md` / `MEMO.md` / `Global_Agent_Rules.md`，确认这里是跨 Agent 思路库，不是正式执行目录。
- 对 `04_参考与引用` 做完整文件清单和行数盘点；Router 之外的参考资料约 43 个文本文件、6772 行。
- 对 EVE、ONE_SPARK、ROVxEVE 采用“读 `.md` 真源、核对 `.txt` 重复源”的方式；不把重复 `.txt` 当第二套规则。
- 对 Grok 语音草案按文件末尾自我校核处理：前文作历史草案，末尾“Claude 实测校核与对齐”优先。

## 可吸收模式

1. EVE：`README + 00 总纲 + 编号用途标签 + 部署提示词`，适合 Ananke AI 工具层。
2. ONE_SPARK：`00_index + 01_operating + 10_global_prompt + M1-M8 模块`，适合 Router 的模块化索引和全局提示词。
3. ROVxEVE：导演包硬字段、逐张制片、母图门禁、UI/构图差异矩阵，适合 image-character-production bundle。
4. `06_rubin-memory`：Markdown truth source、retrieval-policy、trigger cards、candidate 不自动升级，适合 Router 的记忆底座。
5. `luobin-ananke`：烧瓶小人人格、七宗罪双面雷达、冷水后给行动驱动，适合补足 Ananke AI 工具层的“谁在判断”。

## 已吸收进 Router 的具体经验

- EVE 的“用途标签 + 先读 README/00 + 按任务加载必要文件”被转成 `classes/`、`cards/`、`bundles/` 与 `Ananke_通用框架/README.md`。
- EVE 的“身份锁定、九段提示词、冲突检测、自检清单”被转成 `image-character-production` 与 `chatgpt-imagegen` 的来源路由，不复制原提示词全文。
- ONE_SPARK 的“流水线工位、单一全局提示词、模块替换、运维记录”被转成 `00_DECK_INDEX.md`、`90_TEST_CASES.md` 和 AI 工具层单提示词部署。
- ONE_SPARK 的“默认不等、不问太多、先交成品、必要时说明默认值”只适合内容产线；Router 没有把它升级成所有任务通用规则。
- ROVxEVE 的“导演包不是批量 prompt，Codex 逐张制片、逐张看图、逐张验收”被转成 `image-character-production` bundle 的核心门禁。
- ROVxEVE 的“观众学到什么、固定项/变量项、镜头差异矩阵、UI 载体、矩形 UI 正平可读”适合作为图文产线卡，不适合作为普通执行任务规则。
- Grok 语音草案的“费用来源先确认、OAuth 不等于零消耗、先短样再长文、声音克隆需授权”被纳入工具安全思路；真实调用仍指向 `x-grok-build`。
- `luobin-ananke` 的“烧瓶小人观察者、七宗罪双面雷达、行为诊断不做人身攻击、冷水后必须给正面驱动”已补入 `Ananke_通用框架/07_人格`、`04_卡组`、`06_自检`、`09_部署`。前一版 Ananke 框架漏掉人格内核，这一点已修正，但仍需真实用例验证后才能转正。

## 标记为过期或只能作背景

- `2026-06-21_Grok语音功能与调用Skill草案.md`：文件末尾已自我纠正，真相源应以 `~/.codex/skills/x-grok-build/` 为准；本文件保留为背景素材，不作执行接口。
- Grok 草案前文的 `80+ voices / 28 languages` 已被文件末尾校核推翻；当前只把 `eve/ara/rex/sal/leo` 与 `20 languages` 作为已校核口径，仍需以正式 skill 和当前官方能力复核。
- Grok 草案中的直接 `curl https://api.x.ai/v1/tts` 路线需要 xAI API Key；Robin 当前已验证路径是 Hermes OAuth -> `x-grok-build`，不要照前文 curl 当可执行入口。
- ONE_SPARK 旧名“城市松弛感”和 Classic Neg 已废，当前以 `ONE SPARK` 与 `Classic Chrome` 为准。
- `ONE_SPARK_交友Hooks内容系统/` 与历史 `/Users/robin/AltmanCodex/ONE_SPARK` 不是同一个路由语境：前者是交友 Hooks / 私人号文案 / Classic Chrome 内容系统；后者在 memory verification 中出现为社媒巡航、浏览器状态、账号体检、Skill 引入研究。Router 的 `one-spark-framework` card 只指向前者，不能因看到 `ONE_SPARK` 四个字就装备交友文案流水线。
- ONE_SPARK `00_index.md`、`M1_persona.md`、`M5_platforms.md` 存在少量编码乱码字符；不影响 Router，后续维护 ONE_SPARK 时可顺手修正文案。
- ROVxEVE `08_部署_ROVxEVE_项目提示词ChatGPT.md` 的知识库清单里 `00/01/02/03` 出现重复条目；不影响本 Router，但后续维护 ROVxEVE 时应清理。
- ROVxEVE `01_必读_ROVxEVE_总规则选题与系列规划.md` 正文标题残留 `07_CORE_...`，应视为旧标题残留，不改变当前文件名的路由地位。
- EVE 框架里 `.md` 与 `.txt` 双份内容并存；当前不删除，Router 只指向 `.md` 真源。
- `.DS_Store` 是系统噪音，不作为资料。

## 对 Router 的补充要求

- 必须加入 `OpenClaw` agent adapter，原方案缺失。
- AI 工具层放在 `Rubin-Ananke-Router/Ananke_通用框架/`，不单独散落到外层。
- AI 工具层根目录扁平，只保留必要文件和一个全局提示词。
- Agent 层保留完整 cards/bundles/agents，用于执行、验证和回写。
- `Ananke_通用框架` 必须显式承接 `luobin-ananke`，否则只剩判断流程，没有人格承重。
