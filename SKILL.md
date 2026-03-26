---
name: resume
description: Restore conversation context after /compact by reading checkpoint and memory files. Use when the user says "resume", "继续", "where were we", or at the start of a new session after compaction.
allowed-tools: Read, Glob, Grep, Bash
argument-hint: "[optional focus area]"
---

# Resume After Compaction

Restore conversation state that was saved by `/brace-compact` before context compaction.

## Steps

### 1. Load checkpoint

Read `.claude/resume.md` if it exists. This is the primary context source.

If it doesn't exist, skip to step 2 and work with memory only.

### 2. Load memory

Read the project's memory index file (`MEMORY.md` in the memory directory). Cross-reference with the checkpoint to build a complete picture.

### 3. Check recent git history

```bash
git log --oneline -10
```

```bash
git diff --stat HEAD~3..HEAD
```

This shows what changed recently, filling gaps the checkpoint might miss.

### 4. Check environment

If the checkpoint mentions active processes, servers, or background tasks, verify they're still running:

```bash
# Examples — adapt based on what the checkpoint says
pm2 status 2>/dev/null
ss -tlnp | grep -E '3000|8080|7701' 2>/dev/null
```

### 5. Synthesize and report

Output a structured summary:

```
🔄 Context restored

📌 Last task: {what was being worked on}
📍 Left off at: {specific state}
🧠 Key context:
  - {critical fact 1}
  - {critical fact 2}
  - {etc.}

▶️ Next steps:
  1. {first action}
  2. {second action}

⚙️ Environment: {status of running services}
```

If `$ARGUMENTS` specifies a focus area, prioritize that area in the summary and suggest relevant next actions.

### 6. Confirm with user

Ask: "这是上次的进度，要从哪里继续？" to let the user confirm or redirect.

Do NOT start working until the user confirms.
