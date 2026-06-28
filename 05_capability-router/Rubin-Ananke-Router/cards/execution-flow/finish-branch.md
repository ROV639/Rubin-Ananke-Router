# Card｜finish-branch

> 来源：agent=claude-code-mainmac（2026-06-29 新建指针卡）；client=claude-code-vscode；machine=mainmac；created=2026-06-29 CST；thread=unknown

## 何时装备

任务收尾、阶段封板、版本/分支需要固化、Robin 要求"先到这里"或进入下一阶段。

## 必读来源

- `_System/superpowers` 下的 finishing-a-development-branch 流程（位置：Brainstorm `_System/superpowers/` 或 Codex plugin `openai-curated/superpowers/`）
- 当前项目的 Git 工作流约定（主分支策略、PR 模板、commit 规范）
- `../cards/verification-handoff/current-memory-writeback.md`（收尾必装）

## 执行动作

1. 跑完 `verification-before-finish`：文件实际存在、命令实际通过、未完成项已标注。
2. 决定归宿：合并 / PR / 留分支 / 丢弃。
3. 写 commit message 或 PR 描述：含"解决了什么 + 没解决什么 + 下一个接力点"。
4. 回写 `CURRENT.md`（接力文件）和/或 `memory-inbox`（长期经验）。
5. 通知下游 Agent / Robin：状态凭证 + 接力入口。

## 禁止事项

不要把"代码能跑"当封板；不要省略未完成项；不要忘记回写接力就直接说"完成"；不要在没有真实验证凭证时声称合并/PR 已开。

## 完成信号

分支归宿有 Git 状态凭证 + CURRENT/memory 回写已完成 + 下游 Agent 可从接力文件直接续上。