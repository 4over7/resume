# resume

Restore previous session context in a new Claude Code conversation. Pick up where you left off.

在新的 Claude Code 会话中恢复上一次的工作上下文，无缝继续。

> Pairs with [`/brace-compact`](https://github.com/4over7/brace-compact) for seamless context recovery.

## Problem

Starting a new Claude Code session means the AI has no memory of your previous work. You'd have to re-explain everything — what you were doing, what decisions were made, what's left to do.

`/resume` reads checkpoint files saved by `/brace-compact` and restores the AI's full understanding of your project state.

新开 Claude Code 会话时，AI 对之前的工作一无所知。`/resume` 从 `/brace-compact` 保存的检查点文件中恢复上下文，让 AI 无需你重新解释就能继续工作。

## Install

```bash
claude install github:4over7/resume
```

> Also install the companion: `claude install github:4over7/brace-compact`

## Usage

At the start of a new session:

```
/resume
```

Focus on a specific area:

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

## What it restores | 恢复内容

1. **Checkpoint** (`.claude/resume.md`) — task state, critical context, next steps
2. **Memory** (`MEMORY.md`) — project knowledge, decisions, debugging conclusions
3. **Git history** — recent changes for additional context
4. **Active plans** (`.claude/plans/`) — implementation progress
5. **Environment** — verifies running processes mentioned in checkpoint

## Companion: [brace-compact](https://github.com/4over7/brace-compact)

`resume` restores. [`brace-compact`](https://github.com/4over7/brace-compact) saves. Install both:

```bash
claude install github:4over7/brace-compact
claude install github:4over7/resume
```

Before ending a session, run `/brace-compact` to save your work state.
