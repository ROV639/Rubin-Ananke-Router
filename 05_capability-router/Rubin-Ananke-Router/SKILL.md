---
name: rubin-ananke-router
description: Use when Robin tasks involve project memory, thinking frameworks, search, image/video/media tools, publishing, system debugging, verification, cross-agent handoff, or multiple possible skills across Codex, Claude Code, OpenClaw, and Hermes.
---

# Rubin-Ananke-Router

> 来源：agent=codex-mainmac；client=Codex Desktop；machine=mainmac；created=2026-06-29 CST；thread=unknown

## 目标

先判断任务该装备哪些能力，再执行。Router 不替代原 skill；它只告诉 Agent 该读哪张 card、哪个 bundle、哪个原始来源。

## 固定动作

1. 识别任务类型：接力、搜索、严肃分析、Ananke 诊断、生图、视频/媒体、发布、系统排障、skill 治理、记忆写入。
2. 优先选择一个 bundle；没有合适 bundle 时选 2-5 张 cards。
3. 读取 `00_DECK_INDEX.md` 中对应条目，再读 bundle/card。
4. 按 card 的“必读来源”回到原 skill、项目 AGENTS、CURRENT、06_rubin-memory 或项目框架。
5. 完成前执行验收；涉及持续状态时回写 `CURRENT.md` 或 memory inbox。

## 选择顺序

```text
用户明确指定
> 当前项目 AGENTS/CURRENT/MEMO
> Rubin-Ananke-Router bundle/card
> 单个 skill 默认规则
> 普通助手判断
```

## 禁止

- 不把 Router 当百科读完整目录。
- 不把 candidate/草稿记忆当 active 规则。
- 不让 Ananke 默认审判所有任务。
- 不让 Serenity 替代事实核查。
- 不复制原 skill 全文到 card。
- 不声称完成文件、发布、排期、生成图片，除非有真实验证。

## 最小输出

执行前如需要说明，简短列出：

```text
已装备：
- bundle/card

将读取：
- 来源文件或 skill
```

普通小任务可不显式输出，但内部仍应完成路由判断。
