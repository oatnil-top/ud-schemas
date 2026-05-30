# code-reviewer

> Review code changes for bugs, security issues, and best practices

**Category**: Development
**Author**: UnDercontrol
**Tags**: `development`, `code-review`, `quality`

## What it does

This skill instructs an agent to perform thorough code reviews covering correctness, security, performance, readability, error handling, and test coverage. Findings are grouped by severity (critical / warning / suggestion) with concrete fix suggestions.

## Install

```bash
ud market install skill code-reviewer
```

Or manually:
```bash
curl -sL https://raw.githubusercontent.com/oatnil-top/ud-schemas/main/community/skills/code-reviewer/skill.yaml | ud apply -f -
```

## Usage

After installing, use the skill directly or assign it to an agent:

```bash
# Use as a prompt
ud prompt code-reviewer

# Assign to an agent
ud apply -f - <<EOF
apiVersion: ud/v1
kind: Agent
spec:
  name: my-reviewer
  skills: [code-reviewer, ud-cli]
  triggers: [on_mention]
  prompt: "Review PRs assigned to you."
EOF
```
