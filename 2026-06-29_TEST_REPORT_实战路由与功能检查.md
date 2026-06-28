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
- 已改为：当前 public 供检查，检查完成后可再改回 private。

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
- `Ananke_通用框架/`：README + 00-07 + 09，10 个文件。

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
- 检查完成后，如果 Robin 要恢复私有，应把仓库 visibility 改回 private。

## 总结

本轮检查不是只看文件存在；已经按外部访问、根入口、五大核心系统、Router 引用、6 个真实任务场景、Ananke 人格承接、media-tools 入口逐项测试。

当前结论：仓库可以交给其他人或其他 Agent 检查。
