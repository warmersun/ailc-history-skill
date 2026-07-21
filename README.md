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
└── skills/
    └── ailc-history/
        ├── SKILL.md                      # Parent tutor + orchestration (entry point)
        └── references/
            ├── worker-brief.md           # Subagent load order + output contract
            ├── wolfram-recipes.md        # Sole home for WL evaluator rules + recipes
            ├── grounding.md              # Visual matrix, art, AI illustration, honesty
            ├── rivers-natural-earth.md   # Natural Earth centerlines → thick Wolfram rivers
            ├── reply-format.md           # Learner-facing structure (parent teaching turns)
            ├── pedagogy.md               # Context pass, sources, art, flow
            └── primary-sources.md        # Letter/source deep-dives, editions, verified quotes
```

Parent tutors from `SKILL.md`. Subagents are told (in `delegate_task` context) to `skill_view` `worker-brief.md`, then `wolfram-recipes.md` / `grounding.md` as needed — they do not load the full tutor skill body.

## Requirements

| Dependency | Role |
|------------|------|
| **Wolfram MCP** | Entity resolution, maps, timelines, conflict/event graphs |
| **skills** toolset | `skill_view` for worker brief and recipes (parent and children) |
| **made-to-stick** (optional) | Sticky storytelling technique (SUCCESS); skill still works without it |

If Wolfram is unavailable, the skill falls back to careful prose and does not invent map image URLs.

## Install (Hermes)

Register this repo as a [Hermes skills tap](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills#publishing-a-custom-skill-tap), then install the skill. Use the `owner/repo` form — not a full GitHub URL.

```bash
hermes skills tap add warmersun/ailc-history-skill
hermes skills install warmersun/ailc-history-skill/skills/ailc-history --category ailc
```

After that, `/ailc-history` is available in chat. To pick up upstream changes later:

```bash
hermes skills update ailc-history
```

`tap add` only registers the source; `install` is what copies `SKILL.md` and `references/` into `~/.hermes/skills/`. Prefer the install identifier above over `hermes skills search` — custom taps are easy to miss in search (Hermes may skip GitHub when its index is available, and `--source github` can time out while scanning the large default taps).

Ensure Wolfram MCP is configured (`WolframContext`, `WolframAlpha`, `WolframLanguageEvaluator` or equivalent).

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

Canonical list and evaluator rules live in [`skills/ailc-history/references/wolfram-recipes.md`](skills/ailc-history/references/wolfram-recipes.md). Names are always resolved with free-form / typed interpreters — never hand-written canonical entity IDs.

## Design principles

- **Learner-facing only** — no pedagogy jargon or orchestration labels in replies  
- **Compute over guess** — maps and timelines from Wolfram when geography or chronology matter  
- **DRY references** — WL rules only in wolfram-recipes; shared visuals/honesty in grounding; workers get a thin brief  
- **Curate** — few strong artifacts beat long dumps  
- **Label reconstructions** — AI images are modern impressions, never substitutes for dated maps  

## License

[MIT](LICENSE) — Copyright (c) 2026 Sic
