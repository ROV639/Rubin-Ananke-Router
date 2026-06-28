# Rubin-Ananke-Router

> 来源：agent=codex-mainmac；client=Codex Desktop；machine=mainmac；created=2026-06-29 CST；thread=unknown

## 这是什么

Rubin-Ananke-Router 是 Robin 的全 Agent 能力卡组系统。它不替代原 skill、项目规则或记忆库，只负责在任务开始时判断该装备哪些能力。

适用对象：
- Agent 层：Codex、Claude Code、OpenClaw、Hermes。
- AI 工具层：Claude 项目、ChatGPT 项目、Grok 项目，见 `Ananke_通用框架/`。

## 核心原则

1. Router 只选装备，不复制原 skill 全文。
2. 单个能力叫 card；一组任务能力叫 bundle。
3. Agent 先选 bundle/card，再读取对应原文或 skill。
4. 简单任务单选，复杂任务多选。
5. 完成前必须验证，必要时回写 `CURRENT.md` 或候选记忆。

## 文件结构

```text
README.md
SKILL.md
00_DECK_INDEX.md
classes/                 # 7 大类
cards/                   # 单张能力卡
bundles/                 # 任务装备包
agents/                  # Codex / Claude Code / OpenClaw / Hermes 差异
Ananke_通用框架/          # Claude / ChatGPT / Grok 可上传轻量包
90_TEST_CASES.md
91_SOURCE_AUDIT.md
```

## 使用方式

Agent 收到任务后：

```text
用户任务
-> 读 SKILL.md
-> 从 00_DECK_INDEX 选 1 个 bundle 或 2-5 张 cards
-> 读取卡片列出的原始来源
-> 执行
-> 验收
-> 回写接力或候选记忆
```

不要每次读完整目录。先读 `SKILL.md` 和 `00_DECK_INDEX.md`，命中场景后再读对应 bundle/card。

