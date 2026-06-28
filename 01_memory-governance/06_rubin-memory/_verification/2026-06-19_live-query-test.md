# 2026-06-19_live-query-test

> `rubin_memory.sh` 稳定入口的实操测试记录。目标：用 Robin 真实会问的问题跑一轮查询，发现排序/过滤问题后当场自优化。

## 命令入口

```yaml
wrapper: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_workspace/scripts/rubin_memory.sh
backend: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_workspace/scripts/rubin_memory_index.py
index: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_System/tests/rubin-memory-index/rubin_memory.sqlite
```

用法：

```bash
_workspace/scripts/rubin_memory.sh build
_workspace/scripts/rubin_memory.sh validate
_workspace/scripts/rubin_memory.sh query "Buffer 发布前检查" --scope publishing --status active --top-k 5
```

## 构建验证

```text
indexed_chunks=330
```

`validate` 已通过，输出 219 行三视角验证结果。

---

# 实操问题

## Q1：Buffer 国际线发布前要确认什么？

命令：

```bash
_workspace/scripts/rubin_memory.sh query "Buffer 国际线发布前要确认什么 API key 媒体 URL" --scope publishing --status active --top-k 5
```

Top 3：

1. `30_retrieval-policy.md` / `Buffer 发布任务`
2. `01_initial-memory.md` / `E2. Buffer 国际线定位`
3. `20_conflict-index.md` / `敏感信息风险`

判断：通过。

## Q2：MiniMax 什么时候用？

命令：

```bash
_workspace/scripts/rubin_memory.sh query "MiniMax 什么时候用 TTS 音乐 生图 搜索" --scope tool --status active --top-k 5
```

Top 3：

1. `01_initial-memory.md` / `D1. MiniMax 使用边界`
2. `01_initial-memory.md` / `C5. Mavis / MiniMax memory`
3. `30_retrieval-policy.md` / `查询 MiniMax 应该怎么用`

判断：通过。

## Q3：Brainstorm 能不能写代码、脚本、素材？

命令：

```bash
_workspace/scripts/rubin_memory.sh query "Brainstorm 可以写代码脚本素材吗" --scope project --status active --top-k 5
```

Top 3：

1. `30_retrieval-policy.md` / `查询 Brainstorm 能不能写代码`
2. `01_initial-memory.md` / `E3. Brainstorm 文件落点`
3. `01_initial-memory.md` / `A2. Brainstorm 是跨 Agent 共识空间`

判断：通过。

## Q4：OpenClaw 自动晋升经验写到哪里？

命令：

```bash
_workspace/scripts/rubin_memory.sh query "OpenClaw 自动晋升 HEARTBEAT 新经验写到哪里" --source-system openclaw --status active --top-k 5
```

Top 3：

1. `20_conflict-index.md` / `OpenClaw 自动晋升含噪`
2. `01_initial-memory.md` / `F1. 自动记忆不能直接晋升`
3. `sources/openclaw.md` / `禁止导入`

判断：通过。

## Q5：当前社媒矩阵主阵地是谁？

命令：

```bash
_workspace/scripts/rubin_memory.sh query "现在社媒矩阵主阵地 小红书 抖音 B站 旗舰" --scope content --status active --top-k 5
```

Top 3：

1. `01_initial-memory.md` / `E1. 社媒矩阵当前口径`
2. `30_retrieval-policy.md` / `查询社媒矩阵 / 平台打法`
3. `20_conflict-index.md` / `B站旗舰旧结论被覆盖`

判断：通过。

## Q6：Claude/Codex/Hermes/OpenClaw/Mavis 记忆源在哪里？

命令：

```bash
_workspace/scripts/rubin_memory.sh query "Claude Codex Hermes OpenClaw Mavis 记忆源在哪里" --scope ops --status active --top-k 8
```

Top 命中包含：

- `README.md` / `定位`
- `10_source-index.md` / `Mavis / MiniMax`
- `sources/mavis-minimax.md` / `有用内容`
- `10_source-index.md` / `可读内容`

判断：基本通过。因为查询同时命中“Claude/Codex/Hermes/OpenClaw 分工重复”，`20_conflict-index.md` 排名仍靠前；但 Top 8 已能召回 `10_source-index.md`。后续如要更精准，可为“记忆源在哪里”做专用 wrapper 或 query template。

---

# 本轮自优化

## 1. wrapper 参数可用性

新增：

```text
_workspace/scripts/rubin_memory.sh
```

支持：

- `build`
- `validate`
- `query "..." [filters]`

## 2. FTS + vector fallback

问题：某些中文查询只有 query template 命中，相关正文段没有进入候选。

修复：查询不再只取 FTS 命中行；先计算 FTS BM25，再对全部 chunks 计算本地 vector rerank，避免 FTS 漏召回。

## 3. scope 斜杠展开

问题：`scope: tool/media` 不会被 `--scope tool` 命中。

修复：`contains_any()` 会把 `tool/media` 展开为 `tool` 和 `media`，也支持 `a + b` 这类写法。

## 4. source-index 查询加权

问题：“记忆源在哪里”会召回分工冲突，而不是优先 source index。

修复：当 query 包含“记忆源 / 哪里 / source / locations”时，对 `10_source-index.md` 和 `source_card` 加权。

## 当前结论

`rubin_memory.sh` 已经可以实操：

- 能构建索引。
- 能跑三视角验证。
- 能查常见问题。
- 能用 `scope/status/source-system/memory-type/top-k` 控制召回。
- 不改 shell 配置，不安装全局命令，不接外部服务。

## 下一步

建议先不要接 EverOS，先让 Robin/Agent 用这个 wrapper 跑 1–2 天真实查询；如果“记忆源在哪里”这类查询仍觉得不够准，再加预设 query templates 或 alias。