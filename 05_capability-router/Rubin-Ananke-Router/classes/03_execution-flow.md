# 03 Execution Flow｜执行流程

> 来源：agent=codex-mainmac；client=Codex Desktop；machine=mainmac；created=2026-06-29 CST；thread=unknown

目标：让 Agent 不跳步、不抢答、不假完成。

优先来源：
- `_System/superpowers`
- 项目 `AGENTS.md`
- 当前平台的 tool/skill 规则

第一版 cards：
- `clarification-gate`
- `plan-before-execute`
- `debug-loop`
- `verification-before-finish`
- `finish-branch`

边界：流程卡只决定怎么做，具体工具和项目规则仍回源文件。

