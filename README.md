# ud-schemas

Open JSON Schema definitions for [UnDercontrol](https://undercontrol.dev) resource file formats.

[![JSON Schema Draft 2020-12](https://img.shields.io/badge/JSON%20Schema-draft--2020--12-blue)](https://json-schema.org/draft/2020-12/json-schema-core)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

## What's included

### Markdown resources (`.md` — frontmatter + body)

| Schema | Description |
|--------|-------------|
| [`task.v1.schema.json`](./task.v1.schema.json) | Task — YAML frontmatter (title, status, tags, deadline) + markdown body |
| [`note.v1.schema.json`](./note.v1.schema.json) | Note — YAML frontmatter (task_id) + markdown body, attached to a parent task |

### YAML resources (`.yaml` — k8s-like envelope)

| Schema | Description |
|--------|-------------|
| [`resource.v1.schema.json`](./resource.v1.schema.json) | Generic envelope — `apiVersion` + `kind` + `metadata` + `spec` |
| [`board.v1.schema.json`](./board.v1.schema.json) | Kanban board — columns with queries and auto-actions |
| [`budget.v1.schema.json`](./budget.v1.schema.json) | Budget — planned spending with recurring plans and adjustments |
| [`account.v1.schema.json`](./account.v1.schema.json) | Account — financial account with balance |
| [`expense.v1.schema.json`](./expense.v1.schema.json) | Expense — spending transaction linked to budget/account |
| [`income.v1.schema.json`](./income.v1.schema.json) | Income — money received from various sources |

## File format overview

UnDercontrol uses **two file formats**, determined by extension:

- **`.md`** — Tasks and Notes. Markdown with YAML frontmatter. Discriminator: `task_id` in frontmatter means Note, otherwise Task.
- **`.yaml`** / **`.yml`** — All other resources. k8s-like structure with `apiVersion`, `kind`, `metadata`, `spec`.

### Task file (`.md`)

```markdown
---
title: Ship v1 release
status: in-progress
tags:
  - release
  - urgent
deadline: 2025-06-01
---

Final checklist before the v1 launch:

- [ ] Run full test suite
- [ ] Update changelog
- [ ] Tag release
```

### Note file (`.md`)

```markdown
---
task_id: a1b2c3d4-e5f6-7890-abcd-ef1234567890
---

Discussed deployment strategy with the team.
Decided to go with a blue-green deployment approach.
```

### YAML resource file (`.yaml`)

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

Multi-document YAML (multiple resources in one file, separated by `---`):

```yaml
apiVersion: ud/v1
kind: Account
spec:
  name: Checking
  balance:
    amount: 5000.00
    currency: USD
---
apiVersion: ud/v1
kind: Account
spec:
  name: Savings
  balance:
    amount: 20000.00
    currency: USD
```

See the [`examples/`](./examples/) directory for complete example files.

## Usage

### Validate with `check-jsonschema` (Python)

```bash
pip install check-jsonschema

check-jsonschema --schemafile task.v1.schema.json your-task.md.json
```

### Validate with `ajv` (Node.js)

```bash
npm install -g ajv-cli ajv-formats

ajv validate -s task.v1.schema.json -d your-task-frontmatter.json --spec=draft2020
```

### Editor integration

Most editors with YAML/JSON Schema support can use these schemas for autocompletion and validation. Point your editor's schema setting to the raw file URL:

```
https://raw.githubusercontent.com/user/ud-schemas/main/task.v1.schema.json
```

## Versioning

Schemas follow a `v{N}` versioning scheme (e.g., `task.v1.schema.json`). Breaking changes to a schema will result in a new version number (v2, v3, etc.). Non-breaking additions (new optional fields) may be added within the same version.

## License

[MIT](./LICENSE)

---

Part of the [UnderControl](https://undercontrol.dev) project.
