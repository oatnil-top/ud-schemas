# ud-schemas

Open JSON Schema definitions and community registry for [UnDercontrol](https://undercontrol.dev).

[![JSON Schema Draft 2020-12](https://img.shields.io/badge/JSON%20Schema-draft--2020--12-blue)](https://json-schema.org/draft/2020-12/json-schema-core)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

This repository serves two purposes:

1. **Schema definitions** — Machine-readable JSON Schemas that define every resource type in UnDercontrol (tasks, notes, boards, agents, skills, etc.)
2. **Community registry** — A shared marketplace where users publish and install skills, prompts, and agent configurations

## Quick Start

```bash
# Install a community skill into your UnDercontrol workspace
ud market install skill code-reviewer

# Browse all available community packages
ud market list

# Or install manually without the ud CLI
curl -sL https://raw.githubusercontent.com/oatnil-top/ud-schemas/main/community/skills/code-reviewer/skill.yaml | ud apply -f -
```

## Schemas

UnDercontrol uses **two file formats** — Markdown with YAML frontmatter for tasks and notes, and k8s-style YAML envelopes for everything else. These schemas define the structure and validation rules for each format.

### Markdown resources (`.md` — frontmatter + body)

| Schema | Description | Example |
|--------|-------------|---------|
| [`task.v1`](./task.v1.schema.json) | Task with status, tags, deadline + markdown body | [task-example.md](./examples/task-example.md) |
| [`note.v1`](./note.v1.schema.json) | Note attached to a parent task | [note-example.md](./examples/note-example.md) |
| [`comment.v1`](./comment.v1.schema.json) | Comment anchored to task description or notes | — |

### YAML resources (`.yaml` — k8s-like envelope)

| Schema | Description | Example |
|--------|-------------|---------|
| [`resource.v1`](./resource.v1.schema.json) | Generic envelope: `apiVersion` + `kind` + `metadata` + `spec` | — |
| [`board.v1`](./board.v1.schema.json) | Kanban board with columns, queries, and auto-actions | [board-example.yaml](./examples/board-example.yaml) |
| [`budget.v1`](./budget.v1.schema.json) | Budget with recurring plans and adjustments | [budget-example.yaml](./examples/budget-example.yaml) |
| [`account.v1`](./account.v1.schema.json) | Financial account with balance | [account-example.yaml](./examples/account-example.yaml) |
| [`expense.v1`](./expense.v1.schema.json) | Spending transaction linked to budget/account | [expense-example.yaml](./examples/expense-example.yaml) |
| [`income.v1`](./income.v1.schema.json) | Income from various sources | [income-example.yaml](./examples/income-example.yaml) |
| [`project.v1`](./project.v1.schema.json) | Project with cwd, git remote, and board bindings | — |
| [`workspace.v1`](./workspace.v1.schema.json) | Workspace session with task, daemon, and agent bindings | — |

### AI & automation entities

| Schema | Description |
|--------|-------------|
| [`skill.v1`](./skill.v1.schema.json) | Capability prompt that defines what a user or agent can do |
| [`prompt.v1`](./prompt.v1.schema.json) | Instruction template with `{{taskId}}` variable substitution |
| [`agent.v1`](./agent.v1.schema.json) | Autonomous AI worker with soul prompt, skills, and trigger conditions |
| [`agent-cli.v1`](./agent-cli.v1.schema.json) | Launch command configuration for agents in workspace sessions |
| [`daemon.v1`](./daemon.v1.schema.json) | Background process that connects via SSE and spawns agent sessions |
| [`scheduled-job.v1`](./scheduled-job.v1.schema.json) | Scheduled job with cron expression, timezone, and typed payload |

## File Format Overview

### Task (Markdown)

```markdown
---
title: Ship v1 release
status: in-progress
tags: [release, urgent]
deadline: 2025-06-01
---

Final checklist before the v1 launch:

- [ ] Run full test suite
- [ ] Update changelog
- [ ] Tag release
```

### Note (Markdown)

```markdown
---
task_id: a1b2c3d4-e5f6-7890-abcd-ef1234567890
---

Discussed deployment strategy with the team.
Decided to go with a blue-green deployment approach.
```

### YAML Resource (k8s-style)

```yaml
apiVersion: ud/v1
kind: Board
spec:
  name: Sprint Board
  columns:
    - name: Backlog
      query: "status:todo"
    - name: In Progress
      query: "status:in-progress"
    - name: Done
      query: "status:done"
```

### Skill (YAML)

```yaml
apiVersion: ud/v1
kind: Skill
spec:
  name: code-reviewer
  description: "Review code for bugs, security, and best practices"
  tags: [development, code-review]
  content: |
    You are a thorough code reviewer. When given a diff...
```

### Agent (YAML)

```yaml
apiVersion: ud/v1
kind: Agent
spec:
  name: pr-reviewer
  display_name: "PR Reviewer"
  skills: [code-reviewer, ud-cli]
  triggers: [on_mention, on_assign]
  prompt: |
    You review pull requests assigned to you...
```

Multiple resources can be combined in a single file using `---` separators. See the [`examples/`](./examples/) directory for complete examples.

### Create vs Update

All resources follow a declarative kubectl-style pattern:

- **No `id` in metadata** — creates a new resource
- **`id` present in metadata** — updates the existing resource (supports UUID prefix matching)

```bash
# Create a new task
cat task.md | ud apply -f -

# Update an existing task (id in frontmatter)
ud describe task abc123 -o apply | ud apply -f -
```

## Community Registry

The [`community/`](./community/) directory is a shared registry where the community publishes and discovers skills, prompts, and agent configurations.

### Browse

| Type | Count | Browse |
|------|-------|--------|
| Skills | 4 | [community/skills/](./community/skills/) |
| Prompts | 1 | [community/prompts/](./community/prompts/) |
| Agents | 1 | [community/agents/](./community/agents/) |
| Bundles | — | [community/bundles/](./community/bundles/) |

### Using the CLI

```bash
# List all community packages
ud market list

# Filter by type or category
ud market list --type skill
ud market list --category development
ud market list --tag testing

# Search by keyword
ud market search "code review"

# View package details
ud market show skill code-reviewer

# Install into your workspace
ud market install skill code-reviewer
ud market install prompt standup-summary
```

### Manual Install (without `ud market`)

```bash
# Fetch and apply directly from GitHub
curl -sL https://raw.githubusercontent.com/oatnil-top/ud-schemas/main/community/skills/code-reviewer/skill.yaml | ud apply -f -
```

### Private Registries

Point to a custom registry (e.g., a private fork for your organization):

```bash
export UD_MARKET_REGISTRY=https://raw.githubusercontent.com/myorg/ud-skills-internal/main/community
ud market list
```

### Contributing

We welcome community contributions! See [CONTRIBUTING.md](./CONTRIBUTING.md) for the full guide. The short version:

1. Export your skill: `ud describe skill my-skill -o apply > skill.yaml`
2. Remove the `metadata` section (IDs are instance-specific)
3. Add a `README.md` explaining what it does
4. Update the `_index.yaml` with your entry
5. Open a Pull Request

## Validation

### Python (`check-jsonschema`)

```bash
pip install check-jsonschema
check-jsonschema --schemafile task.v1.schema.json your-task.md.json
```

### Node.js (`ajv`)

```bash
npm install -g ajv-cli ajv-formats
ajv validate -s task.v1.schema.json -d your-task-frontmatter.json --spec=draft2020
```

### Editor Integration

Most editors with YAML/JSON Schema support can use these schemas for autocompletion and validation. Point your editor's schema setting to the raw file URL:

```
https://raw.githubusercontent.com/oatnil-top/ud-schemas/main/task.v1.schema.json
```

## Versioning

Schemas follow a `v{N}` versioning scheme (e.g., `task.v1.schema.json`). Breaking changes result in a new version number (v2, v3, etc.). Non-breaking additions (new optional fields) may be added within the same version.

## License

[MIT](./LICENSE)

---

Part of the [UnDercontrol](https://undercontrol.dev) project.
