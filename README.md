# AILC History

**AI Learning Companion — History**: an agent skill for interactive history tutoring grounded in the [Wolfram Knowledgebase](https://www.wolfram.com/knowledgebase/) via Wolfram MCP.

It turns open-ended questions about empires, wars, people, and periods into maps, timelines, conflict graphs, period art, and sticky stories — without dumping dry lists or inventing map URLs.

## What it does

- Places a topic in **world context** (when, where at scale, who held power, who was at war)
- Builds **historical maps and timelines** from Wolfram (dated borders, battles, person lifespans)
- Surfaces **primary sources**, period art/coins, and carefully labeled AI reconstructions when useful
- Keeps the chat **fast**: dispatch parallel Hermes sub-agents first (they inherit Wolfram + `skill_view`), then high-level framing while maps/timelines finish in the background
- Stays honest about missing data, contested historiography, and myth vs record

## Repository layout

```text
.
├── SKILL.md                      # Parent tutor + orchestration (entry point)
└── references/
    ├── worker-brief.md           # Subagent load order + output contract
    ├── wolfram-recipes.md        # Sole home for WL evaluator rules + recipes
    ├── grounding.md              # Visual matrix, art, AI illustration, honesty
    ├── rivers-natural-earth.md   # Natural Earth centerlines → thick Wolfram rivers
    ├── reply-format.md           # Learner-facing structure (parent teaching turns)
    └── pedagogy.md               # Context pass, sources, art, flow
```

Parent tutors from `SKILL.md`. Subagents are told (in `delegate_task` context) to `skill_view` `worker-brief.md`, then `wolfram-recipes.md` / `grounding.md` as needed — they do not load the full tutor skill body.

## Requirements

| Dependency | Role |
|------------|------|
| **Wolfram MCP** | Entity resolution, maps, timelines, conflict/event graphs |
| **skills** toolset | `skill_view` for worker brief and recipes (parent and children) |
| **made-to-stick** (optional) | Sticky storytelling technique (SUCCESS); skill still works without it |

If Wolfram is unavailable, the skill falls back to careful prose and does not invent map image URLs.

## Install

Copy or clone this skill into your agent’s skills directory (name as required by your host, e.g. `ailc-history`):

```bash
git clone https://github.com/warmersun/ailc-history-skill.git
# then link or copy into your skills path
```

Ensure the host can load `SKILL.md` and has Wolfram MCP configured (`WolframContext`, `WolframAlpha`, `WolframLanguageEvaluator` or equivalent).

## Usage

Trigger when the learner wants to explore history, for example:

- “Roman Empire around 100 AD”
- “Timeline of the Thirty Years’ War”
- “What was happening in the world when the Ming dynasty fell?”
- “Map of the Ottoman Empire in 1520”
- `/ailc-history`

Typical turn shape:

1. `delegate_task` fan-out (each child context mandates `skill_view` of worker-brief + wolfram-recipes)  
2. High-level framing in plain language after the handle returns  
3. Later synthesis in the reply format: headed sections, full sentences that gloss terms, short tables when useful, one strong visual, a next hook  

## Entity types (Wolfram)

Canonical list and evaluator rules live in [`references/wolfram-recipes.md`](references/wolfram-recipes.md). Names are always resolved with free-form / typed interpreters — never hand-written canonical entity IDs.

## Design principles

- **Learner-facing only** — no pedagogy jargon or orchestration labels in replies  
- **Compute over guess** — maps and timelines from Wolfram when geography or chronology matter  
- **DRY references** — WL rules only in wolfram-recipes; shared visuals/honesty in grounding; workers get a thin brief  
- **Curate** — few strong artifacts beat long dumps  
- **Label reconstructions** — AI images are modern impressions, never substitutes for dated maps  

## License

[MIT](LICENSE) — Copyright (c) 2026 Sic
