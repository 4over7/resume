# resume

A Claude Code skill that restores conversation context after `/compact`.

> **Part of the compact-resume cycle** — pairs with [`/brace-compact`](https://github.com/4over7/brace-compact) to give Claude Code seamless context recovery across compactions.

## Problem

After `/compact`, Claude Code loses detailed conversation context. The `/resume` skill reads checkpoint files saved by [`/brace-compact`](https://github.com/4over7/brace-compact) and restores the AI's understanding of what you were working on.

## Install

```bash
claude install github:4over7/resume
```

> Also install the companion skill: `claude install github:4over7/brace-compact`

## Usage

After `/compact`:

```
/resume
```

Or focus on a specific area:

```
/resume focus on database migration
```

Or just say "resume" or "继续" in natural language.

## Complete workflow

```
Working...
  → /brace-compact          # save context (companion skill)
  → /compact                 # compress conversation
  → /resume                  # restore context (this skill)
  → Continue working         # seamless!
```

## What it restores

1. **Checkpoint** (`.claude/resume.md`) — task state, critical context, next steps
2. **Memory** (`MEMORY.md`) — project knowledge, decisions, debugging conclusions
3. **Git history** — recent changes for additional context
4. **Environment** — verifies running processes mentioned in checkpoint

## Companion: [brace-compact](https://github.com/4over7/brace-compact)

`resume` restores context. [`brace-compact`](https://github.com/4over7/brace-compact) saves it.

Install both to complete the cycle:

```bash
claude install github:4over7/brace-compact
claude install github:4over7/resume
```

Before `/compact`, run `/brace-compact` to save your session state.
