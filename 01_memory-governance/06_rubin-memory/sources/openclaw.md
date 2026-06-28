# OpenClaw Memory Source Card

```yaml
source_system: openclaw
root: /Users/robin/.openclaw/
status: active-source
sensitivity: mixed
import_policy: curated summaries only; no sqlite/jsonl dump by default
```

## 结构

OpenClaw 记忆包含：

- `~/.openclaw/memory/*.sqlite`：全局/Agent sqlite 记忆。
- `workspace-*/MEMORY.md`：workspace 长期精选。
- `workspace-*/memory/YYYY-MM-DD.md`：短期日记/原始事件。
- `agents/*/sessions/sessions.json`：session 索引。
- `*.jsonl` / `.trajectory.jsonl`：会话轨迹。

## 有用内容

- 长期在线任务经验。
- 浏览器/社媒/发布操作经验。
- Eve/Buffer 交接规则。
- 自动晋升机制的设计启发。

## 禁止导入

- sqlite 全量 dump。
- jsonl session 全文。
- Telegram/Feishu metadata 原文。
- HEARTBEAT、模型切换、临时状态噪音。
- 未审核自动晋升内容。

## 常见任务入口

- 长期观察经验：读 `30_retrieval-policy.md` OpenClaw 长期观察 / 自动记忆任务模板。
- 候选写入：读 `memory-inbox/README.md`，写入 `memory-inbox/openclaw/`。
- 自动晋升审计：读 `20_conflict-index.md` 第 6 条，不把自动晋升直接当 active。

## 当前裁定

OpenClaw 是最接近长期在线 Agent 的记忆系统，但自动晋升含噪。它的机制值得借鉴，内容必须人工筛选后才能进入初始化记忆。