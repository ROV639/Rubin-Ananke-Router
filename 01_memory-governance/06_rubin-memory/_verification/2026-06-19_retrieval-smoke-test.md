# 2026-06-19_retrieval-smoke-test

> 第一轮模拟检索验证：检查 `06_rubin-memory` 是否能按任务只读相关记忆，而不是全量读取。

## 测试规则

- 固定加载：`02_core-consensus.md`。
- 每题最多读取 Top K 3–5 个条目。
- 不打开原始大文件，除非摘要不足。
- 不读取敏感配置值。

---

# Q1：当前 MiniMax 应该怎么用？

## 应读取文件

1. `02_core-consensus.md` 第 9 条。
2. `01_initial-memory.md` D1、D2。
3. `20_conflict-index.md` 第 4 条。
4. `sources/mavis-minimax.md`。

## Top K 记忆

1. MiniMax 主动用于 TTS、音乐/BGM、视频理解、音频理解、STT。
2. MiniMax 生图只有 Robin 明确点名时使用；未指定工具的生图优先 ChatGPT/Codex。
3. 搜索/识图非必要不用 MiniMax，资讯搜索优先 api-search-media-router/Tavily/multi-search。
4. MiniMax/mmx 配置中存在敏感 key 字段，不能导入或输出值。

## 是否需要打开原始源

一般不需要。只有要执行具体 MiniMax 命令时才打开 `minimax-token-plan` skill 或 `api-search-media-router`。

## 冲突判断

多处重复但方向一致；以最新全局规则和 skill 文档为准。

---

# Q2：Brainstorm 里能不能写代码或素材？

## 应读取文件

1. `02_core-consensus.md` 第 2 条。
2. `01_initial-memory.md` A2、E3。
3. `sources/brainstorm.md`。
4. 必要时读 Brainstorm `Global_Agent_Rules.md`。

## Top K 记忆

1. Brainstorm 是思路沟通、方案讨论、决策沉淀空间。
2. 这里只产 Markdown。
3. 代码、脚本、媒体、最终交付物回到 AltmanCodex / AgentProject / 对应项目工作区。
4. 决定写 `02_决定与共识/`，完整方案写 `01_思路与方案/`，草稿写 `03_草稿与待定/`。

## 是否需要打开原始源

如果是要实际写入 Brainstorm 内文件，建议打开 `Global_Agent_Rules.md` 确认目录落点。

## 冲突判断

Rubin-Studio 根规则说最终资产库不生产；Brainstorm 子目录规则是例外，但只允许思路/共识 Markdown。

---

# Q3：Claude/Codex/Hermes/OpenClaw/Mavis 的记忆源分别在哪里？

## 应读取文件

1. `10_source-index.md`。
2. `00_full-memory-extract.md` 各系统总览。
3. `sources/*.md`。

## Top K 记忆

1. Claude Code：`/Users/robin/.claude/projects/*/memory/`。
2. Codex：`/Users/robin/.codex/memories/`。
3. Hermes：`/Users/robin/.hermes/memories/MEMORY.md`、`USER.md`、`SOUL.md`。
4. OpenClaw：`/Users/robin/.openclaw/workspace*/MEMORY.md`、`memory/*.md`、sessions、sqlite。
5. Mavis/MiniMax：`/Users/robin/.minimax/memory/`、`/Users/robin/.minimax/agents/*/memory/`、MiniMax skills。

## 是否需要打开原始源

只有当任务需要系统原始上下文时才打开具体源；默认不读全库。

## 冲突判断

原始系统各自独立，`06_rubin-memory` 只做来源索引和精选摘要，不替代它们。

---

# Q4：如果 Agent 发现新经验，应该写到哪里？

## 应读取文件

1. `02_core-consensus.md` 第 3、4 条。
2. `README.md` 写入规则。
3. `01_initial-memory.md` A3、A4。
4. `30_retrieval-policy.md` 禁止策略。

## Top K 记忆

1. 新经验先写入 `memory-inbox/<source-system>/`。
2. 候选必须等待去重、冲突检查、敏感信息检查和人工/治理确认。
3. 候选不能自动改 core consensus。
4. 自动总结、OpenClaw 晋升、Codex rollout 等都只能作为候选。

## 是否需要打开原始源

不需要，除非要判断是否已存在重复记忆。

## 冲突判断

所有系统的自动记忆都不能直接成为 active consensus。

---

# Q5：哪些记忆不能导入初始化记忆？

## 应读取文件

1. `20_conflict-index.md`。
2. `00_full-memory-extract.md` “不建议导入初始化记忆的内容”。
3. `01_initial-memory.md` B 区安全规则。
4. `30_retrieval-policy.md` 默认 exclude。

## Top K 记忆

1. API Key、Token、Cookie、SSH 密码、账号认证值、完整账号 ID。
2. sqlite 全量内容、jsonl session 全文、Telegram/Feishu metadata 原文。
3. plugin cache、research_raw、archive、deprecated、过期文档。
4. 历史 task state、cwd、thread id、rollout 原文。
5. HEARTBEAT、模型切换临时片段、未审核自动晋升内容。

## 是否需要打开原始源

不需要。若要做安全审计，只报告字段名和路径，不输出值。

## 冲突判断

安全规则优先级最高；任何疑似敏感内容默认不导入。

---

# Q6：查询 2026-06-17 社媒矩阵共识时，当前主阵地是谁？

## 应读取文件

1. `02_core-consensus.md` 第 1 条。
2. `01_initial-memory.md` E1、E1.1、E2。
3. `21_2026-06-17-consensus-audit.md` 总体裁定。
4. `20_conflict-index.md` 第 10、11 条。
5. 具体账号任务再读平台方法论对应段。

## Top K 记忆

1. 当前上位共识以 AI 内容工作室 v2 和小红书 AI 视觉变现旗舰为准。
2. 小红书是变现主阵地；Garfield2025 是 AI 视觉实操中长视频/深度图文旗舰。
3. 抖音是基本盘、涨粉和钩子验证阵地；ONCELUO2023 是 AI 判断基本盘。
4. `指南方针` 中 “B站 = 旗舰主场” 已被后续 v2 覆盖；B站当前是轻维护观察，不是旗舰。
5. 平台机制、扶持政策、Buffer channel/API 细节执行前要按日期复核。

## 是否需要打开原始源

一般不需要。只有要做具体账号运营方案、发布排期或平台机制判断时，才打开对应 2026-06-17 原文并复核当前平台事实。

## 冲突判断

存在明确冲突：`指南方针` 的 B站旗舰结论被 v2 和小红书旗舰决策覆盖。检索应优先返回 `21_2026-06-17-consensus-audit.md` 和 conflict-index 的裁定。

---

# Q7：Buffer 国际线发布前要检查什么？

## 应读取文件

1. `01_initial-memory.md` E2。
2. `30_retrieval-policy.md` Buffer 发布任务模板。
3. `20_conflict-index.md` 第 1 条敏感信息风险。
4. `21_2026-06-17-consensus-audit.md` Buffer 接入说明与国际线规划段。

## Top K 记忆

1. Buffer 是 IG/X/Threads 国际线轻量分发层，不是主阵地。
2. Buffer 不抢小红书、抖音、公众号主产能。
3. 发布前必须确认账号、正文、媒体 URL、发布时间、是否立即发布。
4. API key 不进文档、不进 Git、不进聊天，只能使用环境变量或既定凭据链路。
5. 评论/互动不靠 Buffer，Buffer 只做发布/排期。

## 是否需要打开原始源

只有执行真实发布、删除、批量排期或检查 channel 映射时才打开 Buffer 原始决定文件；一般策略判断不需要。

## 冲突判断

安全规则优先级最高。任何缺少账号、正文、媒体 URL、时间或立即发布确认的请求，都不能直接执行发布。

---

# 验证结论

## 成功点

- 已能通过 `02_core-consensus.md` 固定加载最短共识。
- 已能通过 `10_source-index.md` 定位各系统原始记忆。
- 已能通过 `20_conflict-index.md` 排除敏感、过期、噪音和重复规则。
- 已能通过 `01_initial-memory.md` 获取初始化精选。
- 不需要一次性读取全部原始 memory。

## 待改进

- `00_full-memory-extract.md` 是人工摘要级完整抽取，不是逐条 dump；如果 Robin 要进一步查漏，可按系统继续扩展每个 source card。
- 尚未安装 EverOS，也未验证真实 BM25/vector/filter 索引效果。
- OpenClaw sqlite 和 session jsonl 未展开，后续如需深入需单独授权和脱敏策略。

## 下一步建议

1. Robin 先对比 `00_full-memory-extract.md` 与 `01_initial-memory.md`，确认是否遗漏关键记忆。
2. 若方向正确，再把 `memory-inbox` 候选模板补齐。
3. 之后再决定是否安装 EverOS，只索引 `06_rubin-memory` 摘要层。