# poker-dealer

> 德州扑克荷官 — AI-powered Texas Hold'em dealer in UnDercontrol

**Category**: Other (Game)
**Author**: UnDercontrol
**Tags**: `game`, `poker`
**Required Skills**: `texas-holdem` (community), `ud-cli` (builtin)

## What it does

An AI poker dealer that runs Texas Hold'em games inside UnDercontrol tasks. Mention `@poker-dealer` on any task to start a game against 4 AI opponents with distinct play styles:

- **Alice** — Aggressive (lots of raises, rarely folds)
- **Bob** — Tight (only plays strong hands)
- **Carol** — Loose (plays many hands, loves to call)
- **Dave** — Balanced (standard strategy)

The game runs through task notes (one per hand) and comments (player actions). The dealer uses Python scripts for fair random dealing and accurate hand evaluation.

## Install

```bash
# Install the required skill first
ud market install skill texas-holdem

# Then install the agent
curl -sL https://raw.githubusercontent.com/oatnil-top/ud-schemas/main/community/agents/poker-dealer/agent.yaml | ud apply -f -
```

## How to play

1. Create a task (or use any existing one)
2. Mention `@poker-dealer` in a comment — the dealer starts a new game
3. The dealer deals cards and shows your hand
4. Reply with your action:
   - `c` — Call / Check
   - `r 50` — Raise to 50
   - `b 30` — Bet 30
   - `f` — Fold
   - `a` — All-in
5. The dealer processes AI actions and advances the game
6. Repeat until showdown!

## Game Rules

- 5 players (you + 4 AI), starting chips: 1000 each
- Blinds: 5 / 10, dealer rotates each hand
- Standard Texas Hold'em rounds: Pre-flop, Flop, Turn, River, Showdown
- Eliminated players (0 chips) are removed from the game
