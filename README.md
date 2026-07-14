# AILC History

**AI Learning Companion — History**: an agent skill for interactive history tutoring grounded in the [Wolfram Knowledgebase](https://www.wolfram.com/knowledgebase/) via Wolfram MCP.

It turns open-ended questions about empires, wars, people, and periods into maps, timelines, conflict graphs, period art, and sticky stories — without dumping dry lists or inventing map URLs.

## What it does

- Places a topic in **world context** (when, where at scale, who held power, who was at war)
- Builds **historical maps and timelines** from Wolfram (dated borders, battles, person lifespans)
- Surfaces **primary sources**, period art/coins, and carefully labeled AI reconstructions when useful
- Keeps the chat **fast**: high-level answer first, heavy Wolfram work in parallel background tasks
- Stays honest about missing data, contested historiography, and myth vs record

## Repository layout

```text
.
├── SKILL.md                      # Agent skill definition (entry point)
└── references/
    ├── pedagogy.md               # Context pass, sources, art, flow
    └── wolfram-recipes.md        # Wolfram Language helpers and recipes
```

## Requirements

| Dependency | Role |
|------------|------|
| **Wolfram MCP** | Entity resolution, maps, timelines, conflict/event graphs |
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

1. Immediate high-level framing in plain language  
2. Parallel lookups (continent geopolitics, conflicts, focus map/timeline, era & sources)  
3. Synthesis: one strong visual, a short story or quote, a next hook  

## Entity types (Wolfram)

| Type | Use for |
|------|---------|
| `HistoricalCountry` | Kingdoms, empires, dated borders |
| `MilitaryConflict` | Wars, battles, actor graphs |
| `HistoricalPeriod` | Eras, dynasties, cultural ages |
| `Person` | Notable people and lifespans |
| `HistoricalEvent` | Events, inventions, milestones |
| `GeographicRegion` | Continent-scale maps |

Names are always resolved with free-form / typed interpreters — never hand-written canonical entity IDs.

## Design principles

- **Learner-facing only** — no pedagogy jargon or orchestration labels in replies  
- **Compute over guess** — maps and timelines from Wolfram when geography or chronology matter  
- **Curate** — few strong artifacts beat long dumps  
- **Label reconstructions** — AI images are modern impressions, never substitutes for dated maps  

## License

[MIT](LICENSE) — Copyright (c) 2026 Sic
