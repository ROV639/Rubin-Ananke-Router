# 2026-06-19_local-index-validation

> `06_rubin-memory` 本地可重建索引验证。目标：在不安装 EverOS、不上传文件、不索引原始 Agent memory 的前提下，先把 Markdown 摘要层接成本地 BM25 + scalar filter + lightweight vector rerank 的可运行 MVP。

## 索引实现

```yaml
index_type: local-disposable
truth_source: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/*.md
script: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_workspace/scripts/rubin_memory_index.py
index_path: /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_System/tests/rubin-memory-index/rubin_memory.sqlite
engine:
  bm25: SQLite FTS5 bm25
  scalar_filter: SQLite columns from frontmatter / inferred metadata
  vector_rerank: local hashed token vector, no external embedding API
network: none
sensitive_values_indexed: none intended
```

## 构建结果

命令：

```bash
python3 _workspace/scripts/rubin_memory_index.py build
```

结果：

```text
indexed_chunks=302
index_path=/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_System/tests/rubin-memory-index/rubin_memory.sqlite
```

## 自优化 1：脚本语法修复

第一次构建失败：SQLite FTS5 tokenizer 字符串引号导致 Python `IndentationError`。

修复：把 SQL schema 字符串改为三引号，并改用：

```sql
tokenize = "unicode61 tokenchars '_-./'"
```

## 自优化 2：FTS 查询转义

第一次多视角验证在 `memory-inbox` 查询时失败：FTS 把 `memory-inbox` 解析成列名/操作符，报 `no such column: inbox`。

修复：`make_fts_query()` 对 token 加双引号，避免 `-`、`/` 等字符破坏 FTS 查询。

## 自优化 3：默认排除 verification 层

第二次验证发现 Claude governance 场景会优先召回 `_verification/` 自己的验证记录，污染日常检索。

修复：查询默认排除 `memory_layer=verification`，只有显式传 `--include-verification` 才召回验证日志。

## 自优化 4：段内 metadata 参与过滤

复查时发现 Buffer 发布查询会先召回泛安全段，而不是 `Buffer 发布任务` 模板。原因是索引只使用文件级 frontmatter，忽略正文段落里的 YAML block，导致 `scope: publishing` 过滤不够精确。

修复：为每个 chunk 解析第一个段内 YAML block，并用段内 `scope/source_system/memory_type/status` 覆盖文件级 metadata。复查后 `Buffer 发布任务` 和 `E2. Buffer 国际线定位` 能稳定进入前列。

---

# 三轮验证

## Round 1：Codex 生产视角

查询：

```text
小红书 AI 视觉 变现 抖音 Buffer Codex 生产
```

过滤：

```yaml
status: active
scope:
  - content
  - publishing
```

Top 结果命中：

1. `01_initial-memory.md` E2 Buffer 国际线定位。
2. `02_core-consensus.md` 总目标。
3. `01_initial-memory.md` E1 社媒矩阵当前口径。
4. `01_initial-memory.md` A1 Rubin Studio 当前总锚。
5. `20_conflict-index.md` B站旗舰旧结论被覆盖。

判断：可用。Codex 做生产任务时能召回主锚、小红书/抖音分工、Buffer 边界和 B站冲突裁定。

## Round 2：Claude Code 治理视角

查询：

```text
OpenClaw 自动晋升 candidate memory-inbox core consensus
```

过滤：

```yaml
status: active
scope:
  - ops
```

Top 结果命中：

1. `02_core-consensus.md` 原始记忆不搬家。
2. `02_core-consensus.md` 候选流程。
3. `02_core-consensus.md` 记忆分层。
4. `40_index-schema.md` 文件级 frontmatter。
5. `02_core-consensus.md` 文件总说明。

判断：可用。Claude Code 做治理时能先拿到 core 规则，不会直接改 core；但 OpenClaw 具体噪音规则排名不够靠前，后续可通过 query template 或 source_system=openclaw filter 提升。

## Round 3：其他 Agent 发布视角

查询：

```text
Buffer 发布 确认 账号 正文 媒体 URL API key
```

过滤：

```yaml
status: active
scope:
  - publishing
```

Top 结果命中：

1. `01_initial-memory.md` E2 Buffer 国际线定位。
2. `01_initial-memory.md` B1 敏感值不进入普通记忆。
3. `20_conflict-index.md` 敏感信息风险。
4. `20_conflict-index.md` 平台方法论时效性。

判断：可用。Hermes/OpenClaw 发布场景能优先召回 Buffer 发布确认和敏感信息规则。

---

# 当前结论

本地索引 MVP 已经可运行：

- 能从 `06_rubin-memory` Markdown 摘要层构建可重建 SQLite 索引。
- 能按 `status/scope/source_system/memory_type` 做 scalar filter。
- 能用 SQLite FTS5 做 BM25 风格关键词召回。
- 能用本地 hashed vector 做 lightweight rerank。
- 不需要网络、API key、外部 embedding 服务或 EverOS 安装。

## 仍需注意

- 这个 vector 只是本地 lightweight rerank，不等于真正 embedding。
- 中文分词是简单 bigram，足够 MVP，不是最终检索质量上限。
- 当前只索引 `06_rubin-memory` 摘要层，不索引 `.claude/.codex/.hermes/.openclaw/.minimax` 原始 memory。
- SQLite 索引在 `_System/tests/`，可删除重建，不是 truth source。

## 下一步建议

1. 若继续本地路线：把 query templates 固化为常用命令 wrapper。
2. 若接 EverOS：只把 `06_rubin-memory` 摘要层作为首批导入源，映射 frontmatter 到 scalar filters。
3. 若提升检索质量：再接真正 embedding，但必须先决定本地模型或可信 API。