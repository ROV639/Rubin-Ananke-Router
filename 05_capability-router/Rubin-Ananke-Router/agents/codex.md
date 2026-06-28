# Agent Adapter｜Codex

> 来源：agent=codex-mainmac；client=Codex Desktop；machine=mainmac；created=2026-06-29 CST；thread=unknown

## 定位

Codex 是主生产端，适合本地文件、脚本、媒体链、批处理、浏览器控制、验证和回写。

## 执行规则

- 先读项目 `AGENTS.md` / `CURRENT.md`。
- 能执行就执行，不停在建议。
- 文件改动用可审计方式。
- 图片交付必须 Markdown 预览。
- 完成前运行真实验证。

## 特别注意

Codex 最容易出的问题是把“脚本通过/文件看似存在”当完成；必须装备 `verification-before-finish` 和 `file-delivered-check`。

