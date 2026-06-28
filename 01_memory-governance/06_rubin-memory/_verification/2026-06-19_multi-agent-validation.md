# 2026-06-19_multi-agent-validation

> `06_rubin-memory` 三轮验证与自优化记录。每轮验证视角不同：Codex、Claude Code、其他 Agent。验证目标不是证明“已经完美”，而是暴露检索/写入/边界问题并立即修正。

## 验证对象

- `README.md`
- `01_initial-memory.md`
- `02_core-consensus.md`
- `10_source-index.md`
- `20_conflict-index.md`
- `30_retrieval-policy.md`
- `40_index-schema.md`
- `sources/*.md`
- `_verification/2026-06-19_retrieval-smoke-test.md`

---

# Round 1：Codex 视角验证

## 场景

Codex 接到一个生产型任务：

> “根据今天确定的社媒矩阵，做一条小红书 AI 视觉实操中长视频的脚本，并判断是否需要同步到抖音/Buffer。”

## 验证方式

不用全文读库，只按生产任务路径模拟检索：

1. 固定读 `02_core-consensus.md`。
2. 任务 scope 判定为 `content + publishing + workflow`。
3. 读 `01_initial-memory.md` 的 E1 / E1.1 / E2。
4. 读 `20_conflict-index.md` 的 2026-06-17 B站旗舰冲突和平台时效性。
5. 按需读 `30_retrieval-policy.md` 的“查询社媒矩阵 / 平台打法”。

## 预期召回

- 当前主锚：AI 内容工作室，不是 AI 频道套利。
- 小红书是变现主阵地，Garfield2025 是 AI 视觉旗舰。
- 抖音是基本盘/钩子验证阵地，ONCELUO2023 是判断基本盘。
- Buffer 是国际线轻量分发层，不抢主产能，发布前必须确认。
- Agent 不能现场编观点；选题、观点、钩子和商业取舍需 Robin / 判断链确认。
- B站不是当前旗舰。

## 发现的问题

1. `40_index-schema.md` 定义了字段，但核心文件还没有统一文件级 frontmatter。
2. `30_retrieval-policy.md` 对 Codex 生产任务的读取路径已经有社媒模板，但没有明确“生产型任务不要把 Brainstorm 当素材/代码落点”。
3. Buffer 发布前确认规则在 `01_initial-memory.md` E2 有，但 `30_retrieval-policy.md` 社媒查询模板没有单独提醒 publishing 任务读取 E2。

## 自优化动作

- 已在 `30_retrieval-policy.md` 的社媒矩阵模板中把 E2 列入优先读取。
- 已在 `30_retrieval-policy.md` 增加 Codex 生产型任务模板，明确 Brainstorm 不承载代码、脚本、素材或成片。
- 已为 `README.md`、`01_initial-memory.md`、`02_core-consensus.md`、`10_source-index.md`、`20_conflict-index.md`、`30_retrieval-policy.md` 补文件级 frontmatter。

---

# Round 2：Claude Code 视角验证

## 场景

Claude Code 接到一个架构治理任务：

> “把新发现的 OpenClaw 自动晋升经验写进共享记忆，并考虑是否进入 core consensus。”

## 验证方式

按治理任务路径模拟检索：

1. 固定读 `02_core-consensus.md` 第 3、4、8 条。
2. 读 `README.md` 写入规则。
3. 读 `40_index-schema.md` 的 `candidate` 和 `review_policy`。
4. 读 `20_conflict-index.md` 的 OpenClaw 自动晋升含噪。
5. 读 `10_source-index.md` 的 OpenClaw 段。

## 预期动作

- 不直接修改 `02_core-consensus.md`。
- 新经验写入 `memory-inbox/openclaw/`。
- 标注 `status: candidate`、`source_system: openclaw`、`review_policy: human-review`。
- 不 dump sqlite/jsonl，不导入 HEARTBEAT/metadata/临时状态。

## 发现的问题

1. `memory-inbox/` 只有目录，没有候选模板，Agent 容易写出不一致格式。
2. `40_index-schema.md` 有字段标准，但没有给候选文件的最小模板。
3. `README.md` 说候选必须带来源、状态、置信度，但没有说明文件命名规范。

## 自优化动作

- 已创建 `memory-inbox/README.md`，定义候选文件命名、frontmatter、正文结构和审核流程。
- 已将 `README.md` 写入规则指向 `memory-inbox/README.md`。
- 已在 `40_index-schema.md` 和 `memory-inbox/README.md` 同步候选字段：`status: candidate`、`memory_layer: candidate`、`review_policy: human-review`。

---

# Round 3：其他 Agent 视角验证（Hermes / OpenClaw / Mavis）

## 场景 A：Hermes 发布视角

Hermes 接到：

> “把一条内容发到 Buffer 国际线。”

应召回：

- Buffer 是国际线轻量分发层，不是主阵地。
- 发布前确认账号、正文、媒体 URL、发布时间、是否立即发布。
- API key 不进文档、不进 Git、不进聊天。
- 评论/互动不靠 Buffer。

## 场景 B：OpenClaw 长期在线视角

OpenClaw 接到：

> “长期观察社媒发布后反馈，并把经验写入共享记忆。”

应召回：

- 自动晋升只能作为 candidate。
- 噪音如 HEARTBEAT、模型切换、metadata 不进入 active。
- 写 `memory-inbox/openclaw/`，等待人工审核。

## 场景 C：Mavis / MiniMax 工具视角

Mavis 接到：

> “这条视频要不要用 MiniMax？”

应召回：

- MiniMax 主动用于 TTS、音乐/BGM、视频理解、音频理解、STT。
- MiniMax 生图只有 Robin 明确点名时使用。
- 搜索/识图非必要不用 MiniMax。
- 配置 key 字段存在但不得输出值。

## 发现的问题

1. `sources/hermes.md`、`sources/openclaw.md`、`sources/mavis-minimax.md` 有来源边界，但没有把各自“最常见任务入口”写成简短 lookup。
2. `30_retrieval-policy.md` 有 MiniMax 查询模板，但没有 Hermes Buffer 发布模板和 OpenClaw 观察写入模板。
3. `_verification/2026-06-19_retrieval-smoke-test.md` 覆盖了 MiniMax 与新经验写入，但没有覆盖 Buffer 发布确认。

## 自优化动作

- 已在 `30_retrieval-policy.md` 增加 Buffer 发布任务模板。
- 已在 `30_retrieval-policy.md` 增加 OpenClaw 长期观察 / 自动记忆任务模板。
- 已在 `_verification/2026-06-19_retrieval-smoke-test.md` 增加 Buffer 发布确认题。
- 已在 `sources/hermes.md`、`sources/openclaw.md`、`sources/mavis-minimax.md` 增加“常见任务入口”小节。

---

# 总体结论

当前 `06_rubin-memory` 已能支撑三类 Agent 的按需检索，但为了让不同 Agent 写入格式稳定、发布/观察类任务不漏安全边界，需要补三项：

1. 候选记忆 inbox 模板。
2. Codex / Hermes / OpenClaw 的任务查询模板。
3. 核心文件 frontmatter 标准化。
