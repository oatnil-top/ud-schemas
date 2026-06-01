# generate-learning-html

Generate single-file interactive HTML pages for technical learning content — dark theme, node diagrams, state panels, progressive tabs.

## Install

```bash
ud market install skill generate-learning-html
```

## What It Does

This skill instructs an agent to produce self-contained HTML pages that visualize technical concepts (primarily networking). The pages:

- Use a dark theme (GitHub dark palette)
- Work offline — all CSS/JS inline, no CDN
- Include Chinese annotations
- Support two layout patterns:
  - **Node Flow** (3-column): packet flow visualization with step-by-step animation
  - **Progressive Tabs** (2-column): architecture diagrams that build complexity tab by tab

## Usage

Assign this skill to an agent, then ask it to create a learning page for a topic:

> "Create an interactive HTML page explaining how Linux iptables processes a packet"

The agent will:
1. Write the HTML to `/tmp/`
2. Ask you to preview it (`open /tmp/<name>.html`)
3. Wait for your confirmation before uploading

## Tags

`workflow`, `html`, `learn-network`
