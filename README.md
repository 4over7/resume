# resume

A Claude Code skill that restores conversation context after `/compact`.

## Problem

After `/compact`, Claude Code loses detailed conversation context. The `/resume` skill reads checkpoint files saved by `/brace-compact` and restores the AI's understanding of what you were working on.

## Install

```bash
claude install github:4over7/resume
```

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

## What it restores

1. **Checkpoint** (`.claude/resume.md`) — task state, critical context, next steps
2. **Memory** (`MEMORY.md`) — project knowledge, decisions, debugging conclusions
3. **Git history** — recent changes for additional context
4. **Environment** — verifies running processes mentioned in checkpoint

## Companion: brace-compact

Install brace-compact to save context before compaction:

```bash
claude install github:4over7/brace-compact
```

## Flow

```
Working... → /brace-compact → /compact → /resume → Continue working
```
