# 2026-06-29 TEST REPORT｜实战路由与功能检查

## 测试目标

按真实 Agent 使用场景检查本仓库，而不是只检查文件是否存在。

测试重点：

- 外部是否能访问公开仓库。
- 根入口是否能指导其他 Agent 阅读。
- 五大核心系统是否各有可用入口。
- Router 的 bundle/card 是否没有悬空引用。
- `Ananke_通用框架` 是否真正承接 `luobin-ananke` 人格核心。
- media-tools 是否只保留必要入口，且能被 Router 指向。

## 仓库公开状态

结果：通过。

证据：

- `gh repo view ROV639/Rubin-Ananke-Router` 返回 `visibility=PUBLIC`、`isPrivate=false`。
- GitHub 页面 `https://github.com/ROV639/Rubin-Ananke-Router` 返回 HTTP 200。
- GitHub raw 可读取根 `README.md` 和 `Ananke_通用框架/07_人格...md`。

## 根入口测试

结果：通过，已修正一处状态说明。

检查项：

- `README.md` 说明仓库定位、五个核心系统、先读顺序、Ananke 特别说明。
- `INDEX.md` 给出目录级索引。
- `OPERATIONS.md` 记录首次整理、自查、上架原则和验证结果。

发现并修正：

- 首次上架时 README 和 OPERATIONS 写的是“默认 private”。Robin 要求公开后，这个状态已过期。
- 已改为：当前 public 供检查和后续维护；如后续纳入敏感配置或未脱敏材料，再单独评估是否改回 private。

## 五大核心系统测试

结果：通过。

### 1. memory-governance

入口：

- `01_memory-governance/06_rubin-memory/README.md`
- `01_memory-governance/06_rubin-memory/30_retrieval-policy.md`

测试结论：

- 能明确说明 Markdown truth source、candidate 不自动升级、按 Top K 检索。
- 适合作为总记忆与检索治理底座。

### 2. analysis-reasoning

入口：

- `02_analysis-reasoning/serenity-brainstorm/SKILL.md`

测试结论：

- 能区分 inference mode 和 execution mode。
- 明确要求证据、交叉验证、置信度、冲突源记录。
- 适合投资、账号、市场、内容方向等严肃分析。

### 3. execution-methods

入口：

- `03_execution-methods/superpowers-skills/systematic-debugging/SKILL.md`
- `03_execution-methods/superpowers-skills/verification-before-completion/SKILL.md`

测试结论：

- debugging 要求先 root cause，不能先猜修。
- verification 要求 fresh evidence before claims。
- 适合作为 plan / debugging / verification 方法论来源。

### 4. persona-judgment

入口：

- `04_persona-judgment/2026-06-23_luobin-ananke_persona-system-spec.md`
- `05_capability-router/Rubin-Ananke-Router/Ananke_通用框架/07_人格_Luobin-Ananke_烧瓶小人与七宗罪系统.md`

测试结论：

- 原始人格 spec 存在。
- AI 工具层已承接烧瓶小人、七宗罪双面雷达、行为诊断不做人身攻击、冷水后给正面驱动。

### 5. capability-router

入口：

- `05_capability-router/Rubin-Ananke-Router/SKILL.md`
- `05_capability-router/Rubin-Ananke-Router/00_DECK_INDEX.md`

测试结论：

- Router 说明了先选 bundle/card，再回到原始来源。
- 不替代原 skill，不替代 memory，不把 candidate 当 active。

## Router 完整性测试

结果：通过。

机器检查结果：

- `classes/`：7 个。
- `bundles/`：6 个。
- `agents/`：4 个，含 Codex / Claude Code / OpenClaw / Hermes。
- `cards/`：28 张。
- `Ananke_通用框架/`：README + 00-09，11 个文件，其中 08 是强约束防自由发挥协议。

本地源目录同步测试：

- 源目录：`/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/04_参考与引用/Rubin-Ananke-Router`
- GitHub staging 对应目录：`05_capability-router/Rubin-Ananke-Router`
- 两边 Markdown 内容已对齐。
- `diff -qr` 唯一差异：本地源目录存在 `.DS_Store`，属于系统噪音，未进入 GitHub。
- 已在本地源目录单独跑同一套 Router 完整性、任务路由、Ananke 人格整合测试，结果通过。

Bundle 引用检查：

- `ananke-decision-review`：全部 card 命中。
- `image-character-production`：全部 card 命中。
- `investment-serious-analysis`：全部 card 命中。
- `project-continuation`：全部 card 命中。
- `publishing-schedule`：全部 card 命中。
- `system-debugging`：全部 card 命中。

发现并修正：

- `image-character-production` 原来把 `eve-framework / rov-eve-framework / one-spark-framework` 写在同一行，不利于机器解析。
- 已改成三行独立 card。

## 实战路由场景测试

结果：通过。

已模拟 6 个常见任务：

| 场景 | 应命中 bundle | 结果 |
|---|---|---|
| 继续上次任务 / 接力 | `project-continuation` | 通过 |
| 投资方向严肃分析 | `investment-serious-analysis` | 通过 |
| Ananke 风格审判/诊断 | `ananke-decision-review` | 通过 |
| 人物图 / 封面 / 系列视觉 | `image-character-production` | 通过 |
| Buffer / Hermes / OpenClaw 发布排期 | `publishing-schedule` | 通过 |
| Hermes / OpenClaw / CLI 排障 | `system-debugging` | 通过 |

每个场景均检查：

- bundle 文件存在。
- bundle 内列出的 card 文件存在。
- 关键来源文件存在。

## Ananke 人格整合测试

结果：通过。

检查文件：

- `07_人格_Luobin-Ananke_烧瓶小人与七宗罪系统.md`
- `09_部署_Ananke_全局提示词.md`
- `04_卡组_Ananke_通用能力卡摘要.md`
- `06_自检_Ananke_幻觉与过度审判检查清单.md`

检查项：

- 含 `Luobin-Ananke` / `Luobin Ananke`。
- 含烧瓶小人定位。
- 含七宗罪双面雷达。
- 含“反面诊断 + 正面驱动”。
- 含打行为不打人的边界。
- 含人格自检。

额外回归：

- `04_卡组` 的 Markdown 表格错误已修复，无 `| | --- | --- |` 残留。

## media-tools 测试

结果：通过。

必要入口文件共 13 个，均存在：

- `chatgpt-imagegen/SKILL.md`
- `chatgpt-imagegen/README.md`
- `chatgpt-imagegen/references/prompt-parameters.md`
- `chatgpt-imagegen/references/visual-parameters.md`
- `codex-system-imagegen/SKILL.md`
- `codex-system-imagegen/references/prompting.md`
- `gpt-image-2/SKILL.md`
- `gpt-image-2/README.md`
- `gpt-image-2/README.zh-CN.md`
- `baoyu-image-gen/SKILL.md`
- `baoyu-cover-image/SKILL.md`
- `baoyu-xhs-images/SKILL.md`
- `minimax-image/SKILL.md`

测试结论：

- Router 能通过 `chatgpt-imagegen` card 找到 ChatGPT/Codex 生图入口。
- Router 能通过 `minimax-media` card 区分 MiniMax 音频/视频理解主用场景和 MiniMax 生图显式触发场景。
- media-tools 只放入口资料，没有复制完整大型工具仓库。

## 仍需人工复核

- `Ananke_通用框架` 还需要真实对话样本测试，确认不会退化成毒舌表演。
- `serenity-brainstorm` 的投资/市场分析需要联网或一手材料，不能只在本仓库离线测试。
- OpenClaw / Hermes adapter 的真实运行效果，需要在对应运行环境里再跑一次。
- 当前按公开仓库维护；如果后续加入敏感配置、未脱敏路径或不宜公开的内部材料，再评估是否把仓库 visibility 改回 private。

## 2026-06-29 追加修复与补测

修复项：

- 把 `debug-loop` 接入 `system-debugging` bundle。
- 把 `seven-sins-diagnosis` 接入 `ananke-decision-review` bundle。
- 把 `investment-thesis-check` 接入 `investment-serious-analysis` bundle。
- 把 `minimax-media`、`finish-branch` 等补入 `00_DECK_INDEX.md` P1，避免孤儿卡不可发现。
- 修复 `debug-loop`、`finish-branch`、`investment-thesis-check`、`seven-sins-diagnosis` 的相对路径死链。
- 修复 `Ananke_通用框架/03_路由` 的旧笔误，统一为 `账号/项目去留判断`。
- 在 `Ananke_通用框架/01_必读` 增加 Serenity 三档与 Ananke 四档的置信度合并规则。
- `09_部署` 旧版曾做双复制块同步；现已改为 Claude / ChatGPT / Grok 三个平台各自独立的一键复制区，避免同内容双写漂移。
- 在 `07_人格` 增加原始 spec vs 压缩版对照表，明确哪些内容保留、压缩或转给 Router。
- 在 `one-spark-framework` 和 `91_SOURCE_AUDIT` 明确区分 `ONE_SPARK_交友Hooks内容系统` 与历史 `/Users/robin/AltmanCodex/ONE_SPARK` 社媒巡航研究。
- 新增 `92_ANANKE_REPLAY_TESTS.md`，覆盖 7 个 Ananke/Luobin-Ananke 回放场景。

补测项：

- 本地源目录先跑完整测试，再同步 staging。
- 测试覆盖：bundle 引用、Deck P0/P1、orphan card、相对路径死链、Ananke 复制区一致性、人格关键词、自检项、ONE_SPARK 误装备防护。

最终测试结果：

```text
LOCAL_SOURCE_FULL_TEST_OK
STAGING_FULL_TEST_OK
cards: 28
referenced_cards: 28
orphans: []
Ananke files: 11
copy_blocks: 3
Claude copy block chars: 3373
ChatGPT copy block chars: 3383
Grok copy block chars: 3300
```

仍需人工验证：

- ChatGPT / Claude / Grok 项目指令字段的真实字符上限仍需在对应产品里手动试填；当前建议优先放 Project instructions，不建议塞进普通 Custom Instructions。
- `92_ANANKE_REPLAY_TESTS.md` 是回放测试规格，尚未让真实 AI 工具逐条生成输出并人工评分。

## 总结

本轮检查不是只看文件存在；已经按外部访问、根入口、五大核心系统、Router 引用、6 个真实任务场景、Ananke 人格承接、media-tools 入口逐项测试。

当前结论：仓库可以交给其他人或其他 Agent 检查。

## 2026-06-29 追加：Ananke 通用框架重新架构

触发原因：

- Robin 指出 `00/01/03/05` 主要是在写要求，没有解释“为什么这样判断、每一步逻辑是什么”。
- 原 `09_部署` 是单一全局提示词，不适合 Claude / ChatGPT / Grok 不同工具的使用差异。
- 需要更强约束，防止大模型只看字面、自由发挥、没充分思考就输出审判口吻。

修复项：

- 重写 `00_总纲`：补入思想锚点和完整推理链 `对象澄清 -> 事实分层 -> 代价计算 -> 偏差识别 -> 反证挑战 -> 行动落点`。
- 重写 `01_必读`：把判断公式、证据层级、置信度降级规则、Serenity 合并规则写清楚。
- 重写 `03_路由`：从关键词触发改为任务意图、对象清晰度、证据状态、后果重量四变量路由。
- 重写 `05_模板`：补每个字段为什么存在，防止模板被当普通填空。
- 新增 `08_强约束_Ananke_充分思考与防自由发挥协议.md`：约束跳读、自由发挥、信息不足硬判、七宗罪人格攻击。
- 重构 `09_部署`：改成 Claude / ChatGPT / Grok 三个独立一键复制区，每个复制区可单独工作，不需要拼接多段。
- 修正 `04_卡组` 一处冲突口径：反证不会自动提高置信度；没反证的重大判断必须降级。

补测结果：

```text
ANANKE_LOCAL_TEST_OK
ANANKE_SOURCE_STAGING_TEST_OK
Ananke files: 11
Claude copy block chars: 3373
ChatGPT copy block chars: 3383
Grok copy block chars: 3300
copy blocks: 3
```

测试覆盖：

- `README` 是否列出 `08_强约束` 和三平台部署口径。
- `00/01/03/05` 是否包含思想锚点、推理链、路由变量、字段逻辑。
- `09_部署` 是否有 Claude / ChatGPT / Grok 三个独立复制区。
- 每个复制区是否包含：身份、推理链、启用条件、五面镜头、证据层级、置信度、七宗罪、回答前强制检查、输出模板、信息不足模板、失败输出识别、平台专用约束。
- 是否清除旧的 `textarea + plain` 双写复制口径。
- 是否仍残留旧的“反证自动提高置信度”、旧笔误等冲突文本。

仍需人工验证：

- 需要把 `92_ANANKE_REPLAY_TESTS.md` 的 7 个场景分别交给 Claude / ChatGPT / Grok 实跑并评分。
- 需要确认三个平台项目指令字段的真实长度限制；当前复制区约 3.3k 字符，普通 Custom Instructions 可能仍不适合，优先 Project instructions。
