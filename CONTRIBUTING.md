# Contributing to the UnDercontrol Community Registry

Thank you for sharing your skills, prompts, and agents with the community!

## How to contribute

### 1. Prepare your package

Each package is a directory containing a YAML file and a README:

```
community/skills/my-skill/
├── README.md        # Documentation
└── skill.yaml       # The skill definition
```

**Export an existing skill from your UnDercontrol instance:**
```bash
ud describe skill my-skill -o apply > skill.yaml
```

Then edit the file to remove the `metadata` section (IDs are instance-specific):
```yaml
# Remove this:
# metadata:
#   id: abc123-...

# Keep this:
apiVersion: ud/v1
kind: Skill
spec:
  name: my-skill
  description: "What it does in one line"
  tags: [tag1, tag2]
  content: |
    Your skill content here...
```

### 2. Create a README

Each package needs a `README.md` with:
- Name and description
- Category and tags
- Install instructions
- Usage examples

See existing packages for reference.

### 3. Update the index

Add an entry to the appropriate `_index.yaml`:

```yaml
# community/skills/_index.yaml
entries:
  # ... existing entries ...
  - name: my-skill
    description: "What it does in one line"
    author: your-github-username
    tags: [tag1, tag2]
    category: development  # see categories below
    path: my-skill/skill.yaml
    added: "2026-06-15"  # today's date
```

### 4. Submit a Pull Request

Fork the repo, add your package, and open a PR.

## Package types

| Type | Directory | YAML kind | File name |
|------|-----------|-----------|-----------|
| Skill | `community/skills/` | `Skill` | `skill.yaml` |
| Prompt | `community/prompts/` | `Prompt` | `prompt.yaml` |
| Agent | `community/agents/` | `Agent` | `agent.yaml` |
| Bundle | `community/bundles/` | (multiple files) | various |

## Categories

Use one of these categories in your `_index.yaml` entry:

- `development` — Code review, API design, testing, debugging
- `writing` — Documentation, blog posts, content creation
- `operations` — DevOps, deployment, monitoring, incident response
- `data` — Data analysis, reporting, visualization
- `project-management` — Task management, standup, retrospectives
- `design` — UI/UX design, architecture, system design
- `communication` — Email drafting, meeting notes, status updates
- `other` — Everything else

## Guidelines

- **Name format**: Use lowercase slug format (`my-skill`, not `My Skill`)
- **No hardcoded IDs**: Remove `metadata.id`, `owner_id`, `group_id` from YAML files
- **Original content**: Ensure your contribution is original or properly attributed
- **Keep it focused**: Each skill/prompt should do one thing well
- **Test it first**: Install your package locally (`ud apply -f skill.yaml`) before submitting

## Agent bundles

Agents can bundle the skills they depend on:

```
community/agents/my-agent/
├── README.md
├── agent.yaml
└── skills/
    ├── helper-skill-1.yaml
    └── helper-skill-2.yaml
```

The `ud market install agent my-agent` command will install the agent and all bundled skills.
