---
name: ud-integration
description: "Manage UnDercontrol tasks, notes, and boards via the ud CLI. Pick up tasks from kanban, execute work, report progress, and update status. Use when asked to work on tasks, track progress, update kanban boards, or collaborate through UnDercontrol."
version: 1.0.0
license: MIT
compatibility: Requires ud CLI (see ud.oatnil.com for install) and network access to UD backend.
metadata:
  author: UnDercontrol
  homepage: https://ud.oatnil.top
---

# UnDercontrol Task Management

Manage tasks, track progress, and collaborate via kanban boards using the `ud` CLI. UnDercontrol (UD) provides task/note/board/skill management with a kubectl-style interface.

## Prerequisites

- `ud` CLI installed — see [ud.oatnil.com](https://ud.oatnil.com) for installation options
- Authenticated context configured:

```bash
ud config get-contexts          # list available contexts
ud config use-context <name>    # switch to target workspace
ud get task --limit 1           # verify connection
```

## Core Concepts

| Concept | Description |
|---------|-------------|
| **Task** | Unit of work with title, description, status, tags, deadline |
| **Note** | Progress entry attached to a task (ephemeral updates) |
| **Board** | Kanban board grouping tasks into columns |
| **Skill** | Reusable prompt template stored on the server |
| **Status** | `todo` → `in-progress` → `pending` → `done` |

Partial IDs (first 8 chars) work everywhere. Example: `ud describe task a1b2c3d4`

---

## Session Workflow

When given a task to work on, follow this exact flow:

### 1. Read the Task

```bash
ud describe task <id>
```

This returns **everything**: title, description, all notes, attachments, and links. No need to run separate commands.

### 2. Set Status to In-Progress

```bash
cat <<'EOF' | ud apply -f -
---
id: <task-id>
status: in-progress
title: <keep original title>
tags: [<keep original tags>]
board: <keep original board>
---
<keep or refine original description>
EOF
```

**Important**: `ud apply` replaces entirely — always include ALL fields.

### 3. Write a Session Start Note

```bash
cat <<'EOF' | ud apply -f -
---
task_id: <task-id>
---
## Session Start
- Plan: <what you understood, what you'll do>
- Key files: <files involved>
EOF
```

### 4. Do the Work

Execute the task. After each meaningful step or commit, write a progress note:

```bash
cat <<'EOF' | ud apply -f -
---
task_id: <task-id>
---
## Progress
- <what was done> — commit [<hash-first-8>]
- Next: <what's next>
EOF
```

### 5. Commit with Task Prefix

All git commits must use this format:

```bash
git commit -m "[<task-id-first-8>] <message>"
```

Example: `git commit -m "[a1b2c3d4] fix: login redirect respects next param"`

### 6. Set Status to Pending

When implementation is complete, set status to `pending` (NOT `done` — the human reviews first):

```bash
cat <<'EOF' | ud apply -f -
---
id: <task-id>
status: pending
title: <keep original title>
tags: [<keep original tags>]
board: <keep original board>
---
<updated description with decisions made>
EOF
```

### 7. Write a Session End Note

```bash
cat <<'EOF' | ud apply -f -
---
task_id: <task-id>
---
## Session End
- Accomplished: <what was done>
- Commits: [hash1], [hash2]
- Remaining: <anything left or follow-up needed>
EOF
```

---

## Command Reference

### Reading Tasks and Boards

```bash
# List tasks by status
ud get task --status todo
ud get task --status in-progress

# Full task details (includes notes, attachments, links)
ud describe task <id>

# List boards
ud get board

# View board with columns and tasks
ud describe board <id|name>

# Query board tasks
ud query board <id|name> "status = 'todo'" --limit 20

# Search tasks (full-text across title, description, notes)
ud task grep "<keyword>"

# Natural language query
ud task nl "<question>"
```

### Writing Tasks

```bash
# Create a new task
cat <<'EOF' | ud apply -f -
---
title: Task Title
status: todo
tags: [tag1, tag2]
board: <board-id>
---
Description in markdown.
EOF

# Update existing task (MUST include all fields — replaces entirely)
cat <<'EOF' | ud apply -f -
---
id: <task-id>
title: Updated Title
status: in-progress
tags: [tag1, tag2]
board: <board-id>
---
Updated description.
EOF
```

### Writing Notes

```bash
# Create a note on a task
cat <<'EOF' | ud apply -f -
---
task_id: <task-id>
---
## Note Title
Content here.
EOF

# Update an existing note (include note_id)
cat <<'EOF' | ud apply -f -
---
task_id: <task-id>
note_id: <note-id>
---
## Updated Note
Updated content.
EOF
```

### Search and Discovery

```bash
# Full-text search across title, description, and notes (like grep -r)
ud task grep "<keyword>"

# With filters
ud task grep "<keyword>" --status todo
ud task grep "<keyword>" --tags work
ud task grep "<keyword>" --limit 10
ud task grep "<keyword>" --sort updated_at --order desc
ud task grep "<keyword>" --tags work,urgent
ud task grep "<keyword>" --status todo --status in-progress

# Title-only matching (like find -name)
ud task query "title ILIKE '%<keyword>%'"

# Natural language query (most flexible)
ud task nl "<question>"

# SQL-like query for precise filtering
ud task query "status = 'in-progress' AND tags @> '{work}'"
```

**Search workflow:**
1. Start with `ud task grep "<keyword>"` — searches everything
2. If too many results, narrow with `--status` or `--tags`
3. For title-only, use `ud task query "title ILIKE '%keyword%'"`
4. For details on a match, run `ud describe task <id>`

### Common Queries

```bash
# What am I working on?
ud get task --status in-progress

# What's waiting for review?
ud get task --status pending

# Urgent or deadline tasks
ud task nl "tasks with deadlines in the next 3 days"
ud get task --tags urgent

# Overdue tasks
ud task nl "overdue tasks"

# Recently completed
ud get task --status done --limit 10
```

### Board Overview

```bash
# List all boards
ud get board

# Full board with columns and tasks
ud describe board <id|name>

# Query specific board tasks
ud query board <id|name> "status = 'todo'" --limit 20
```

### Skills (Project Workflows)

```bash
# List available skills
ud get skills

# Read a skill prompt (get workflow instructions)
ud prompt <skill-name>
```

### Task Connections

```bash
# Link tasks as peers
ud task link <id1> <id2>

# Parent-child relationship
ud task link <parent-id> <child-id> --subtask
```

---

## Picking Up Tasks from a Board

When asked to "pick up a task" or "work on the next item":

1. **Browse the board:**
   ```bash
   ud describe board <board-name>
   ```

2. **Pick a `todo` task** — prefer tasks with deadlines or urgent tags

3. **Read it fully:**
   ```bash
   ud describe task <id>
   ```

4. **If requirements are unclear**, ask the human before starting. Do NOT implement vague tasks.

5. **Follow the Session Workflow** (steps 2-7 above)

---

## Answering Workload Questions

When asked "what should I work on?" or "give me a standup summary":

**Prioritization:**
1. Check in-progress tasks first — finish what's started
2. Check urgent/deadline tasks
3. Check the board for prioritized items
4. Suggest the highest-impact item

**Standup summary format:**
1. Done: `ud get task --status done` (recent)
2. In progress: `ud get task --status in-progress`
3. Blocked: `ud task grep "blocked"` or pending tasks

---

## Error Handling

| Symptom | Cause | Fix |
|---------|-------|-----|
| `error: not authenticated` | No valid context | Run `ud config get-contexts` and `ud config use-context <name>` |
| `error: not found` | Wrong task ID | Use `ud task grep` to search |
| `error: network` | Server unreachable | Check connectivity, retry once |
| Apply silently fails | Missing required fields | Always include ALL fields (apply replaces entirely) |

---

## Tips

- **Partial IDs work**: First 8 chars of any UUID is enough
- **Apply replaces entirely**: When updating, always include title, status, tags, board, and description
- **Notes are ephemeral**: Use for progress updates. Refine the task description for permanent decisions.
- **Never set status=done**: Always use `pending` — the human reviews and moves to `done`
- **Use `ud prompt`**: Check for project-specific skills that may have additional workflow instructions
