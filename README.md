# ud-schemas

Open JSON Schema definitions for [UnderControl](https://undercontrol.dev) task and note file formats.

[![JSON Schema Draft 2020-12](https://img.shields.io/badge/JSON%20Schema-draft--2020--12-blue)](https://json-schema.org/draft/2020-12/json-schema-core)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

## What's included

| Schema | Description |
|--------|-------------|
| [`task.v1.schema.json`](./task.v1.schema.json) | Task file format — YAML frontmatter (title, status, tags, deadline) + markdown body |
| [`note.v1.schema.json`](./note.v1.schema.json) | Note file format — YAML frontmatter (task_id) + markdown body, attached to a parent task |

## File format overview

UnderControl uses plain markdown files with YAML frontmatter. This makes files human-readable, diffable, and compatible with any text editor.

### Task file

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

### Note file

```markdown
---
task_id: a1b2c3d4-e5f6-7890-abcd-ef1234567890
---

Discussed deployment strategy with the team.
Decided to go with a blue-green deployment approach.
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
