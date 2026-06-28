---
title: Core Consensus
index_id: core-consensus
memory_layer: core
status: active
scope:
  - global
  - security
  - ops
  - content
source_system:
  - rubin-memory
  - brainstorm
memory_type:
  - consensus
  - security
  - workflow
sensitivity: low
time_sensitivity: current
canonical: true
last_reviewed: 2026-06-19
source_paths:
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/01_initial-memory.md
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/21_2026-06-17-consensus-audit.md
exclude_from_startup: false
---

# 02_core-consensus

> 所有 Agent 可固定加载的最短共识。这里只放当前有效、跨系统通用、低敏、不会因项目切换频繁变化的规则。

## 1. 总目标

Robin 当前做的是 **AI 内容工作室**，不是 AI 频道套利。核心锚点是变现；护城河来自生产系统、判断、审美、人格、自有受众和 IP。当前社媒上位共识以 2026-06-17 的 AI 内容工作室 v2 和小红书 AI 视觉变现旗舰为准：小红书是变现主阵地，抖音是基本盘/涨粉/钩子验证阵地，B站不是当前旗舰。

## 2. Brainstorm 边界

`/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/` 是跨 Agent 思路、方案、共识和记忆治理空间。这里只产 Markdown；代码、脚本、媒体、最终交付物回到对应工程/项目工作区。

## 3. 记忆分层

Consensus 是当前有效规则，必须人工确认。Memory 是历史事实、经验、失败、候选和案例，只能作为依据。任何 Agent 不得把聊天内容或自动总结直接升级为核心共识。

## 4. 候选流程

Agent 发现新经验时，写入 `memory-inbox/<source-system>/`，等待去重、冲突检查、敏感信息检查和 Robin/治理流程确认。候选不能自动改 core consensus。

## 5. 敏感信息

API Key、Token、Cookie、SSH 密码、账号认证值、完整账号 ID 不进入普通记忆。只允许记录文件存在、字段名称、配置类型和脱敏风险说明。

## 6. 高风险动作

删除、覆盖、发布、付款、远程操作、修改密钥、系统级命令必须先确认。确认只覆盖当次明确动作，不自动扩展到相邻高风险动作。

## 7. 读取策略

不要一次性读取全部记忆。默认固定加载本文件；任务识别后按来源、scope、project、status 检索 Top K = 3–5。检索结果优先返回摘要、状态和路径，必要时再打开单个原文。

## 8. 原始记忆不搬家

Claude Code、Codex、Hermes、OpenClaw、Mavis/MiniMax 都保留自己的原始 memory。`06_rubin-memory` 只保存来源索引、冲突索引、初始化精选和治理规则。

## 9. 工具路由

MiniMax 是工具能力，主要用于 TTS、音乐/BGM、视频理解、音频理解、STT。未指定工具的生图优先 ChatGPT/Codex；资讯搜索优先 api-search-media-router / Tavily / multi-search 路线。

## 10. 旧规则处理

archive、deprecated、plugin cache、research_raw、过期文档默认不作为当前规则召回。只有审计历史时才读。