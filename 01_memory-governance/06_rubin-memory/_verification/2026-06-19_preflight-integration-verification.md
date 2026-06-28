---
title: Preflight Integration Verification
index_id: verification-2026-06-19-preflight-integration
memory_layer: verification
status: active
scope:
  - ops
  - project
source_system:
  - rubin-memory
  - codex
  - claude-code
memory_type:
  - verification
  - workflow
  - failure
sensitivity: low
time_sensitivity: current
canonical: false
last_reviewed: 2026-06-19
source_paths:
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/06_rubin-memory/preflight/
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_workspace/scripts/rubin_memory.sh
  - /Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_workspace/scripts/rubin_memory_index.py
  - /Users/robin/AltmanCodex/Eve_Frames/AGENTS.md
  - /Users/robin/AltmanCodex/Rov_Eve/AGENTS.md
  - /Users/robin/AltmanCodex/Paper Moon/AGENTS.md
exclude_from_startup: true
---

# 2026-06-19_preflight-integration-verification

## 完成定义

这轮不能只算“写了规则”。完成必须满足：

1. `06_rubin-memory/preflight/` 有三项目短门禁。
2. `rubin_memory.sh preflight <name>` 能直接输出正文，不带 frontmatter。
3. 本地索引能识别 `memory_layer: preflight`。
4. 三个 AltmanCodex 项目入口 `AGENTS.md` 首屏硬规则直接指向对应 preflight 命令。
5. 三个项目 `AGENTS_VERSION` 更新，确保新线程能检测规则变更。
6. 新增/修改内容经过 fresh verification。

## 自查发现与修正

### 1. schema 漏字段

问题：`40_index-schema.md` 只列到 `source_card | candidate`，但实际新增了 `trigger` 和 `preflight`。

修正：已把 `trigger`、`preflight` 加入 `memory_layer` 枚举，并补充字段语义。

### 2. wrapper 输出方式不稳

问题：`rubin_memory.sh preflight` 初版使用 `sed -n '/^# /,$p'`，能工作，但依赖第一个 Markdown 标题位置；比解析 frontmatter 更脆。

修正：改成用 `$PYTHON_BIN` 去除 frontmatter 后输出正文。

### 3. 入口接入不完整

问题：之前只在 rubin-memory 内创建 preflight，项目入口还没强制读，实际生产时不会自动生效。

修正：已在三项目 `AGENTS.md` 首屏硬规则顶部接入：

- Eve_Frames → `rubin_memory.sh preflight eve-frames`
- Rov_Eve → `rubin_memory.sh preflight rov-eve`
- Paper Moon → `rubin_memory.sh preflight paper-moon`

### 4. 版本未变更

问题：项目 `AGENTS.md` 修改后若不更新 `AGENTS_VERSION`，接力机制不一定提示规则变化。

修正：三项目版本都更新到 `2026-06-19.1`。

## fresh verification

### 索引验证

命令：

```bash
cd '/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵' && bash _workspace/scripts/rubin_memory.sh build && bash _workspace/scripts/rubin_memory.sh validate
```

结果：

```text
indexed_chunks=428
index_path=/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_System/tests/rubin-memory-index/rubin_memory.sqlite
```

`validate` 三组查询均返回 active 结果。

### guard 验证

命令：

```bash
for name in chatgpt-portrait-template api-search-key-rotation altmancodex-guided-question altmancodex-artifact-not-result altmancodex-image-identity-mother; do
  bash _workspace/scripts/rubin_memory.sh guard "$name" | wc -l
done
```

结果：五个 guard 均输出 5 行。

### preflight 验证

命令：

```bash
for name in eve-frames rov-eve paper-moon; do
  bash _workspace/scripts/rubin_memory.sh preflight "$name" | python3 -c 'import sys; data=sys.stdin.read(); print(len(data.splitlines()), data.startswith("---"), "QC 表" in data)'
done
```

结果：

```text
eve-frames: lines=45 has_frontmatter=False has_qc=True
rov-eve: lines=56 has_frontmatter=False has_qc=True
paper-moon: lines=57 has_frontmatter=False has_qc=True
```

### 项目入口验证

命令：

```bash
/opt/homebrew/bin/rg -n "rubin_memory\.sh' preflight|rubin_memory\.sh preflight|preflight (eve-frames|rov-eve|paper-moon)" \
  '/Users/robin/AltmanCodex/Eve_Frames/AGENTS.md' \
  '/Users/robin/AltmanCodex/Rov_Eve/AGENTS.md' \
  '/Users/robin/AltmanCodex/Paper Moon/AGENTS.md'
```

结果：

```text
/Users/robin/AltmanCodex/Rov_Eve/AGENTS.md:16:`bash '/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_workspace/scripts/rubin_memory.sh' preflight rov-eve`
/Users/robin/AltmanCodex/Eve_Frames/AGENTS.md:16:`bash '/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_workspace/scripts/rubin_memory.sh' preflight eve-frames`
/Users/robin/AltmanCodex/Paper Moon/AGENTS.md:15:`bash '/Users/robin/Rubin-Studio/✨Brainstorm - Agent🧵/_workspace/scripts/rubin_memory.sh' preflight paper-moon`
```

## 结论

本轮完成的是“三项目 preflight 从记忆层到项目入口的最小闭环”。

仍未完成的更高等级闭环：让 Codex/Claude Code 在真实生产任务中自动执行这些命令并把结果写入任务目录 QC。那需要在对应生产 skill 或项目工作流里进一步硬接，不只是 AGENTS 首屏提示。
