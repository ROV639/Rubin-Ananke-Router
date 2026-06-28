# Codex Memory Source Card

```yaml
source_system: codex
root: /Users/robin/.codex/memories/
status: active-source
sensitivity: mixed
import_policy: workflow and failure summaries only
```

## 结构

Codex memory 是集中式记忆库：

- `MEMORY.md`：综合偏好、用户画像、项目索引。
- `memory_summary.md`：按 task group 聚合摘要。
- `raw_memories.md`：thread、cwd、task、outcome、keywords、failure 等归并记录。
- `rollout_summaries/`：单次任务或 rollout 总结。

## 有用内容

- 可复用生产 workflow。
- 失败复盘。
- 用户偏好。
- 项目执行经验。
- Skill 生产链和工具路由经验。

## 禁止导入

- 历史任务状态作为当前状态。
- cwd/thread/task id 原文。
- 一次性错误日志全文。
- 未确认自动推断。

## 当前裁定

Codex memory 适合作为生产经验和失败模式来源；不适合作为当前项目状态的权威。涉及项目当前状态时，必须回到项目 `CURRENT.md` / `MEMO.md` 或实际文件。