# Card｜debug-loop

> 来源：agent=claude-code-mainmac（2026-06-29 新建指针卡）；client=claude-code-vscode；machine=mainmac；created=2026-06-29 CST；thread=unknown

## 何时装备

排障卡住超过一次尝试、错误信息反复出现、问题在多个层面（配置/网络/权限/工具）来回漂移。

## 必读来源

- `_System/superpowers` 下的 debugging / systematic-debugging / root-cause-tracing 流程（位置：Brainstorm `_System/superpowers/` 或 Codex plugin `openai-curated/superpowers/`）
- 当前项目 `AGENTS.md` 故障处理约定
- `../bundles/system-debugging.md`（协同 bundle）

## 执行动作

1. 复现：拿到最小可复现条件（输入、命令、环境、版本）。
2. 收证据：日志、stack trace、退出码、最近一次成功状态。
3. 分离变量：每次只改一个变量；改前记录基线。
4. 二分定位：从最可能层往两边切——配置 / 网络 / 权限 / 工具 / 数据。
5. 验证修复：跑同一个复现路径三次，确认不是偶发。

## 禁止事项

不要凭直觉改三处再回头看哪条有效；不要跳过复现直接猜根因；不要相信"我重启过了"这种无凭证的状态报告。

## 完成信号

修复有最小复现命令作证 + 至少三次连续成功 + 失败模式与根因的因果链已写到报告/CURRENT。