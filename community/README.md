# UnDercontrol Community Registry

Browse and install community-contributed skills, prompts, and agents for [UnDercontrol](https://undercontrol.dev).

## Quick Install

```bash
# Install a skill
ud market install skill code-reviewer

# Install a prompt
ud market install prompt standup-summary

# Browse all packages
ud market list
```

Or install manually without `ud market`:
```bash
curl -sL https://raw.githubusercontent.com/oatnil-top/ud-schemas/main/community/skills/code-reviewer/skill.yaml | ud apply -f -
```

## Skills

| Name | Description | Category |
|------|-------------|----------|
| [code-reviewer](skills/code-reviewer/) | Review code changes for bugs, security issues, and best practices | Development |
| [test-writer](skills/test-writer/) | Generate comprehensive test cases from code or requirements | Development |
| [api-designer](skills/api-designer/) | Design RESTful APIs following best practices and conventions | Development |

## Prompts

| Name | Description | Category |
|------|-------------|----------|
| [standup-summary](prompts/standup-summary/) | Summarize a task for daily standup updates | Project Management |

## Agents

*No community agents yet. [Contribute one!](../CONTRIBUTING.md)*

## Bundles

*No bundles yet. [Contribute one!](../CONTRIBUTING.md)*

## Contributing

See [CONTRIBUTING.md](../CONTRIBUTING.md) for how to submit your skills, prompts, and agents.
