# texas-holdem

> 德州扑克玩法规则、发牌脚本、牌力评估、AI决策逻辑

**Category**: Other (Game)
**Author**: UnDercontrol
**Tags**: `game`, `poker`

## What it does

Complete Texas Hold'em poker skill that enables an AI agent to deal and manage poker games inside UnDercontrol tasks. Includes:

- Full game rules (blinds, betting rounds, showdown)
- Python dealing script for fair random card distribution
- Python hand evaluation script (detects all hand ranks from high card to royal flush)
- AI opponent decision logic with 4 distinct play styles (aggressive, tight, loose, balanced)
- Structured output format using task notes for game records

## Install

```bash
ud market install skill texas-holdem
```

Or manually:
```bash
curl -sL https://raw.githubusercontent.com/oatnil-top/ud-schemas/main/community/skills/texas-holdem/skill.yaml | ud apply -f -
```

## Usage

This skill is designed to be used with the [poker-dealer](../../agents/poker-dealer/) agent. Install both:

```bash
ud market install skill texas-holdem
ud market install agent poker-dealer
```

Then mention `@poker-dealer` on any task to start a game!

## Player Controls

| Command | Action |
|---------|--------|
| `c` | Call / Check |
| `ck` | Check |
| `r 50` | Raise to 50 |
| `b 30` | Bet 30 |
| `f` | Fold |
| `a` | All-in |
