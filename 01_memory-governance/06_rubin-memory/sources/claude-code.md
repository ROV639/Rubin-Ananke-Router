# Claude Code Memory Source Card

```yaml
source_system: claude-code
root: /Users/robin/.claude/projects/*/memory/
status: active-source
sensitivity: mixed
import_policy: selective summary only
```

## 结构

Claude Code memory 按项目路径编码分桶。每个项目 memory 目录通常包含：

- `MEMORY.md`：索引。
- 单条 `.md` 记忆：偏好、反馈、项目经验、工具路由。
- 新式条目常带 frontmatter：`name`、`description`、`metadata.type`。

## 有用内容

- 稳定用户偏好。
- 项目规则。
- 工具路由。
- 历史任务沉淀。
- 用户反馈。

## 禁止导入

- 凭证、key、token、账号 ID、SSH 等敏感条目。
- 旧 worklog 全文。
- 已过期项目状态。

## 重要路径

- `/Users/robin/.claude/projects/-Users-robin-Rubin-Studio--Brainstorm---Agent--/memory/`
- `/Users/robin/.claude/projects/-Users-robin-AltmanCodex/memory/`
- `/Users/robin/.claude/projects/-Users-robin-AgentProject/memory/`
- `/Users/robin/.claude/projects/-Users-robin--hermes/memory/`

## 当前裁定

Claude Code memory 可作为偏好和项目经验来源，但不是唯一共识源。涉及跨 Agent 当前共识时，以 Brainstorm `MEMO.md` / `02_决定与共识/` 和 `06_rubin-memory/02_core-consensus.md` 为准。