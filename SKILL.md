---
name: resume
description: Restore conversation context after /compact by reading checkpoint and memory files. Use when the user says "resume", "继续", "where were we", or at the start of a new session after compaction.
allowed-tools: Read, Glob, Grep, Bash
argument-hint: "[optional focus area]"
---

# Resume After Compaction

Restore conversation state that was saved by `/brace-compact` before context compaction. After compact, `/resume` is the entry point for continuing work.

## Steps

Execute ALL steps in order, then output the summary. If a step fails or returns no data, note it and continue to the next step.

### 1. Load checkpoint

Read `.claude/resume.md` if it exists. This is the primary context source — it contains task state, critical context, next steps, files touched, and environment info.

If it doesn't exist, log "No checkpoint found" and continue. The remaining steps can still provide useful context.

### 2. Load memory

Find and read the project's memory index (`MEMORY.md` in the memory directory). The memory directory path is typically shown in the system context.

If `MEMORY.md` exists, scan it for entries relevant to the checkpoint's task. If the checkpoint mentions specific topics (e.g., "database migration", "voice AI"), look for matching memory files.

If no memory directory or `MEMORY.md` is found, skip this step.

### 3. Check recent git history

Run these in parallel:

```bash
git log --oneline -10
```

```bash
git diff --stat HEAD~3..HEAD
```

This shows what changed recently, filling gaps the checkpoint might miss. Compare with the checkpoint's "Files touched" section to see if anything changed since the checkpoint was written.

### 4. Check active plans

Look for plan files in `.claude/plans/`. If any exist, read them to understand the current implementation plan and its progress.

```bash
ls .claude/plans/ 2>/dev/null
```

If a plan exists and relates to the checkpoint task, include its status in the summary.

### 5. Check environment

If the checkpoint's "Active environment" section mentions running processes, servers, or background tasks, verify they're still running. Adapt commands based on what the checkpoint describes:

```bash
# Adapt these based on checkpoint content — do NOT hardcode ports
# Examples:
# pm2 status 2>/dev/null          # if checkpoint mentions PM2
# docker ps 2>/dev/null            # if checkpoint mentions Docker
# ss -tlnp 2>/dev/null             # if checkpoint mentions specific ports
```

If the checkpoint doesn't mention any active environment, skip this step.

### 6. Synthesize and report

Combine information from all sources into a structured summary:

```
🔄 Context restored

📌 Last task: {from checkpoint "Task in progress", or inferred from git/memory}
📍 Left off at: {from checkpoint "Current state"}
🧠 Key context:
  - {from checkpoint "Critical context" — list each item}
📋 Plan: {from .claude/plans/ — status summary, or "none active"}

▶️ Next steps:
  1. {from checkpoint "Next steps"}
  2. {etc.}

⚙️ Environment: {from step 5 — what's running vs what should be}
```

**Priority rules:**
- Checkpoint > Memory > Git history (checkpoint is most authoritative)
- If `$ARGUMENTS` specifies a focus area, prioritize that area and suggest relevant next actions
- If no checkpoint exists, build the summary from memory + git history alone
- Flag any discrepancies (e.g., checkpoint says server should be running but it's not)

### 7. Confirm with user

Ask: "这是上次的进度，要从哪里继续？" to let the user confirm or redirect.

Do NOT start working until the user confirms. The user may want to change direction or correct the restored context.
