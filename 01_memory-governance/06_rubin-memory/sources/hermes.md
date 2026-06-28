# Hermes Memory Source Card

```yaml
source_system: hermes
root: /Users/robin/.hermes/memories/
status: active-source
sensitivity: medium
import_policy: preference and boundary summaries only
```

## 结构

Hermes 主要记忆文件：

- `MEMORY.md`：系统/项目长期记忆。
- `USER.md`：用户偏好。
- `SOUL.md`：Nomos 身份、判断框架、行动边界。

Hermes 工具层有 `memory_tool.py` 和 memory provider，可在会话生命周期中读写记忆。

## 有用内容

- Nomos/OpenClaw 边界。
- Brainstorm 规则。
- 模型路由。
- 高风险动作确认。
- 不编造和信息过期查证规则。
- 社媒发布偏好。

## 禁止导入

- Nomos 专属身份细节作为所有 Agent 规则。
- 临时项目快照。
- 任何敏感字段值。

## 常见任务入口

- Buffer 发布：读 `01_initial-memory.md` E2，再读 `30_retrieval-policy.md` Buffer 发布任务模板。
- 高风险动作：读 `02_core-consensus.md` 第 6 条和 Hermes `SOUL.md` 通用边界。
- Brainstorm 规则判断：以 Brainstorm 本地 `Global_Agent_Rules.md` / `MEMO.md` 为准，Hermes 只作引用。

## 当前裁定

Hermes memory 是判断/发布/矩阵规则的重要来源，但不是所有 Agent 的身份模板。抽取其中通用边界和安全规则即可。