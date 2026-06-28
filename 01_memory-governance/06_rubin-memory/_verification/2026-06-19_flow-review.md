# 2026-06-19_flow-review

> `06_rubin-memory` 全流程复查记录。目标：进入下一步前检查是否有“疤”和 bug；发现问题先自优化，再判断是否可继续。

## 复查范围

- Markdown truth source：`06_rubin-memory/**/*.md`
- 结构化 schema：`40_index-schema.md`
- 候选写入：`memory-inbox/README.md`
- 本地索引脚本：`_workspace/scripts/rubin_memory_index.py`
- 可重建索引：`_System/tests/rubin-memory-index/rubin_memory.sqlite`
- 验证记录：`_verification/*.md`

## 检查 1：脚本语法

命令：

```bash
python3 -m py_compile _workspace/scripts/rubin_memory_index.py
```

结果：通过。

## 检查 2：重建与验证流水线

命令：

```bash
python3 _workspace/scripts/rubin_memory_index.py build
python3 _workspace/scripts/rubin_memory_index.py validate
```

结果：通过。

当前索引统计：

```yaml
chunks: 315
verification_chunks: 75
startup_allowed:
  - 02_core-consensus.md
  - 10_source-index.md
  - 20_conflict-index.md
  - 30_retrieval-policy.md
  - README.md
layers:
  conflict_index: 51
  core: 11
  full_extract: 38
  initial: 30
  retrieval_policy: 32
  schema: 12
  source_card: 34
  source_index: 32
  verification: 75
```

## 检查 3：占位符和敏感模式

扫描项：

- `TODO`
- `TBD`
- `待补`
- `API_KEY=`
- `sk-`
- `Bearer `

结果：未发现命中。

## 检查 4：核心 frontmatter

检查文件：

- `README.md`
- `01_initial-memory.md`
- `02_core-consensus.md`
- `10_source-index.md`
- `20_conflict-index.md`
- `30_retrieval-policy.md`

结果：全部存在文件级 frontmatter。

## 检查 5：verification 层默认排除

验证：同一 OpenClaw governance 查询，在不加 `--include-verification` 时不会召回 `_verification/`；加上时才召回验证日志。

结果：通过。

## 检查 6：定向查询

### Buffer 发布

查询：

```bash
python3 _workspace/scripts/rubin_memory_index.py query 'Buffer 发布 确认 API key' --scope publishing --status active --top-k 5
```

复查前问题：先召回泛安全段，`Buffer 发布任务` 排名不够靠前。

原因：脚本只使用文件级 frontmatter，没有解析段内 YAML block。

修复：为 chunk 增加 `parse_section_metadata()`，用段内 `scope/source_system/memory_type/status` 覆盖文件级 metadata。

修复后前列命中：

1. `30_retrieval-policy.md` / `Buffer 发布任务`
2. `01_initial-memory.md` / `E2. Buffer 国际线定位`
3. `20_conflict-index.md` / `敏感信息风险`

### MiniMax 工具

查询：

```bash
python3 _workspace/scripts/rubin_memory_index.py query 'MiniMax TTS 音乐 生图 搜索' --scope tool --status active --top-k 5
```

前列命中：

1. `01_initial-memory.md` / `D1. MiniMax 使用边界`
2. `01_initial-memory.md` / `C5. Mavis / MiniMax memory`
3. `01_initial-memory.md` / `D2. 搜索与生图优先级`

### OpenClaw 自动晋升

查询：

```bash
python3 _workspace/scripts/rubin_memory_index.py query 'OpenClaw 自动晋升 HEARTBEAT candidate' --source-system openclaw --status active --top-k 4
```

前列命中：

1. `01_initial-memory.md` / `F1. 自动记忆不能直接晋升`
2. `sources/openclaw.md` / `禁止导入`
3. `20_conflict-index.md` / `OpenClaw 自动晋升含噪`
4. `01_initial-memory.md` / `C4. OpenClaw memory`

## 发现并修复的问题

1. FTS tokenizer 引号导致脚本语法错误：已修。
2. `memory-inbox` 中 `-` 破坏 FTS 查询：已对 query token 加引号。
3. verification 日志污染日常检索：已默认排除，显式参数才包含。
4. 段内 metadata 没有参与过滤，导致 Buffer 查询排序不理想：已解析段内 YAML 并覆盖 chunk metadata。

## 当前结论

没有发现阻塞下一步的 bug。当前系统满足：

- Markdown 仍是真相源。
- SQLite 索引可删除重建。
- 默认不索引原始 Agent memory。
- 默认不召回 verification 日志。
- 候选写入有模板。
- 三类视角检索都能命中目标规则。

## 可以进入的下一步

建议下一步不是继续扩 Markdown，而是封装稳定命令入口：

```bash
rubin-memory build
rubin-memory query "..." --scope publishing --status active
rubin-memory validate
```

入口可以先做成 `_workspace/scripts/rubin_memory.sh`，不改 shell 配置、不安装全局命令。