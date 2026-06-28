# 2026-06-19_task-replay-test

> 用本轮之前发生过的真实任务做检索回放。目的：不是只问“能不能查”，而是比较不同试验方式，找出更接近真实 Agent 使用的测试路径。

## 测试对象

```yaml
wrapper: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_workspace/scripts/rubin_memory.sh
index: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_System/tests/rubin-memory-index/rubin_memory.sqlite
raw_results: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/_verification/2026-06-19_task-replay-results.jsonl
```

## 试验方式对比

### 方式 A：自然语言裸查

例：

```bash
rubin_memory.sh query "我要发 Buffer 国际线 发布前检查什么" --status active --top-k 5
```

优点：最接近 Robin 或 Agent 随手问。

缺点：容易同时召回相近但不够聚焦的段落，比如“分工重复”“README 定位”。

适合：快速探索、Robin 临时问。

### 方式 B：带 filter 查询

例：

```bash
rubin_memory.sh query "Buffer 发布 账号 正文 媒体 URL 时间 API key" --scope publishing --status active --top-k 5
```

优点：明显更准，能体现 EverOS 式 scalar filter 精髓。

缺点：需要 Agent 先判断任务 scope。

适合：Claude/Codex/Hermes/OpenClaw 在执行任务前自动检索。

### 方式 C：template 查询

例：

```bash
rubin_memory.sh template buffer-publish
rubin_memory.sh template source-locations
```

优点：最稳定，能把常见任务固化成最佳查询参数。

缺点：需要维护模板表。

适合：高频入口、Agent 启动钩子、命令菜单。

### 方式 D：候选记忆流程测试

例：

```bash
rubin_memory.sh template candidate-write
```

优点：测试的不只是检索，还测试“新经验能不能安全进入 inbox，而不是污染 core”。

缺点：需要后续人工审核闭环。

适合：OpenClaw / Codex / Claude Code 自动总结类任务。

## 用过去任务回放的结果

### 1. EverOS 成熟度判断

自然语言和 filtered 查询都能命中：

- `20_conflict-index.md` / `EverOS 插件成熟度`

结论：通过。当前判断仍是“架构精髓可学，官方接入层不直接照搬”。

### 2. 2026-06-17 社媒共识是否过时

自然语言命中：

- `21_2026-06-17-consensus-audit.md` / `仍有效`
- `20_conflict-index.md` / `B站旗舰旧结论被覆盖`

filtered 查询命中：

- `01_initial-memory.md` / `E1. 社媒矩阵当前口径`
- `02_core-consensus.md` / `总目标`

结论：通过。能区分“小红书当前旗舰”和“B站旧结论被覆盖”。

### 3. Buffer 发布前检查

自然语言命中：

- `21_2026-06-17-consensus-audit.md` Buffer 段
- `01_initial-memory.md` E2
- `30_retrieval-policy.md` Buffer 发布任务

template 优化后命中：

- `30_retrieval-policy.md` / `Buffer 发布任务`
- `01_initial-memory.md` / `E2. Buffer 国际线定位`
- `20_conflict-index.md` / `敏感信息风险`

结论：通过。template 比裸查更适合执行前检查。

### 4. MiniMax 路由

自然语言命中：

- `sources/mavis-minimax.md` / `有用内容`
- `01_initial-memory.md` / `D1. MiniMax 使用边界`
- `02_core-consensus.md` / `工具路由`

filtered/template 命中：

- `01_initial-memory.md` / `D1. MiniMax 使用边界`
- `30_retrieval-policy.md` / `查询 MiniMax 应该怎么用`
- `01_initial-memory.md` / `D2. 搜索与生图优先级`

结论：通过。

### 5. 新经验写到哪里 / 能不能直接改 core

自然语言命中：

- `02_core-consensus.md` / `候选流程`
- `02_core-consensus.md` / `记忆分层`
- `memory-inbox/README.md`

template 命中：

- `02_core-consensus.md` / `候选流程`
- `02_core-consensus.md` / `记忆分层`

结论：基本通过。若要更强调 `memory-inbox/README.md`，后续可给 candidate template 加权。

### 6. 各 Agent 记忆源在哪里

自然语言裸查会召回：

- `20_conflict-index.md` / `Claude / Codex / Hermes / OpenClaw 分工重复`
- `README.md` / `定位`
- `02_core-consensus.md` / `原始记忆不搬家`

filtered/template 查询能命中：

- `10_source-index.md` / `Claude Code`
- `sources/claude-code.md`
- `10_source-index.md` / `Mavis / MiniMax`

结论：这是最能暴露问题的场景。裸查不够准，template 明显更好。

---

# 本轮自优化

## 1. 增加 template 命令

新增：

```bash
rubin_memory.sh template buffer-publish
rubin_memory.sh template minimax-usage
rubin_memory.sh template brainstorm-boundary
rubin_memory.sh template social-matrix
rubin_memory.sh template candidate-write
rubin_memory.sh template source-locations
rubin_memory.sh template everos-maturity
```

## 2. Buffer template 去掉过窄 memory-type filter

问题：`--memory-type workflow` 会漏掉 `01_initial-memory.md` E2，因为 E2 的段内类型是 `consensus`。

修复：`buffer-publish` template 保留 `--scope publishing --status active`，不强行限制 memory type。

## 3. 明确最佳试验方式

当前最好的测试组合是：

1. 先裸查，模拟 Robin / Agent 自然提问。
2. 再 filtered 查，模拟 Agent 判断 scope 后检索。
3. 最后 template 查，固化高频任务入口。
4. 对“新经验”类任务，额外跑 candidate-write 流程。

## 当前判断

这套功能已能用来做真实任务前的记忆检索，但不能只依赖自然语言裸查。真正符合 Rubin 需求的方式是：

```text
Agent 先判断任务类型
→ 选 template 或 filter
→ 召回 Top K
→ 必要时再读原文
→ 新经验只进 memory-inbox
```

## 下一步

最值得做的是候选记忆实操闭环：

1. 模拟一个真实新经验。
2. 写入 `memory-inbox/<source-system>/`。
3. 用索引查到它。
4. 判断是否该晋升到 `01_initial-memory.md`。
5. 不直接改 `02_core-consensus.md`。