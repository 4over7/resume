# resume

在新会话中恢复上一次的工作上下文，无缝继续。

> **Part of the compact-resume cycle** — pairs with [`/brace-compact`](https://github.com/4over7/brace-compact) to give Claude Code seamless context recovery.

## Problem

以下场景都会导致 Claude Code 丢失上下文：
- `/compact` 压缩对话
- 退出会话（重启 Claude Code、加载新配置）
- 会话超时、切换项目、隔天继续

`/resume` 从 `/brace-compact` 保存的检查点文件中恢复上下文，让 AI 无需你重新解释就能继续工作。

## Install

```bash
claude install github:4over7/resume
```

> Also install the companion skill: `claude install github:4over7/brace-compact`

## Usage

新会话开始后：

```
/resume
```

Or focus on a specific area:

```
/resume focus on database migration
```

Or just say "resume" or "继续" in natural language.

## Workflows

**After compact:**
```
/brace-compact → /compact → /resume → Continue working
```

**After exit/restart:**
```
/brace-compact → exit → claude → /resume → Continue working
```

**Next morning:**
```
claude → /resume → Pick up where you left off
```

## What it restores

1. **Checkpoint** (`.claude/resume.md`) — task state, critical context, next steps
2. **Memory** (`MEMORY.md`) — project knowledge, decisions, debugging conclusions
3. **Git history** — recent changes for additional context
4. **Active plans** (`.claude/plans/`) — implementation progress
5. **Environment** — verifies running processes mentioned in checkpoint

## Companion: [brace-compact](https://github.com/4over7/brace-compact)

`resume` restores context. [`brace-compact`](https://github.com/4over7/brace-compact) saves it.

Install both to complete the cycle:

```bash
claude install github:4over7/brace-compact
claude install github:4over7/resume
```

Before ending a session, run `/brace-compact` to save your work state.
