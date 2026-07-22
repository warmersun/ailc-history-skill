# AILC History

**AI Learning Companion — History**: agent skills for history learning grounded in the [Wolfram Knowledgebase](https://www.wolfram.com/knowledgebase/) via Wolfram MCP.

This package ships **two skills**:

| Skill | Mode | Use when… |
|-------|------|-----------|
| **`ailc-history`** | Interactive chat tutor | Live exploration, maps on demand, Socratic Q&A |
| **`ailc-history-lesson`** | Offline lesson author | Prepared multi-chapter markdown package with TOC, profile, ledger, and inline visuals |

Both share the same Wolfram toolset and honesty rules. Lesson authoring **depends on** `ailc-history` references (not duplicated).

## What they do

### `ailc-history` (interactive)

- Places a topic in **world context** (when, where at scale, who held power, who was at war)
- Builds **historical maps and timelines** from Wolfram (dated borders, battles, person lifespans)
- Surfaces **primary sources**, period art/coins, and carefully labeled AI reconstructions when useful
- Keeps the chat **fast**: dispatch parallel Hermes sub-agents first (they inherit Wolfram + `skill_view`), then high-level framing while maps/timelines finish in the background
- Stays honest about missing data, contested historiography, and myth vs record

### `ailc-history-lesson` (prepared package)

- Profiles the learner (level, goals, time budget, constraints)
- Builds a **question ledger** (primary / secondary / rabbit holes / unresolved)
- Writes a navigable lesson under `./output/<slug>/`: `index.md` TOC, numbered chapters with prev/next links, `assets/`, `sources.md`
- Embeds Wolfram maps/timelines and story illustrations **inline** in the chapter files
- Takes as long as needed — optimized for durable self-study files, not chat speed

## Repository layout

```text
.
└── skills/
    ├── ailc-history/
    │   ├── SKILL.md                      # Interactive tutor + orchestration
    │   └── references/
    │       ├── worker-brief.md           # Subagent load order + output contract
    │       ├── wolfram-recipes.md        # Sole home for WL evaluator rules + recipes
    │       ├── grounding.md              # Visual matrix, art, AI illustration, honesty
    │       ├── rivers-natural-earth.md   # Natural Earth centerlines → thick Wolfram rivers
    │       ├── reply-format.md           # Learner-facing structure (parent teaching turns)
    │       ├── pedagogy.md               # Context pass, sources, art, flow
    │       └── primary-sources.md        # Letter/source deep-dives, editions, verified quotes
    └── ailc-history-lesson/
        ├── SKILL.md                      # Lesson package author (batch / offline)
        └── references/
            ├── learner-intake-ledger.md  # Profile the learner before writing
            ├── history-question-ledger.md
            ├── lesson-structure.md       # Chapter arcs by level and time budget
            ├── chapter-template.md       # Nav + guiding questions + visuals
            └── visual-assets.md          # Save/embed maps and story images on disk
```

Interactive parent tutors from `ailc-history/SKILL.md`. Subagents load `worker-brief.md` then recipes via `skill_view` — they do not load the full tutor skill body.

Lesson author loads its own `SKILL.md` plus **sibling** `ailc-history` references (path or `skill_view("ailc-history", …)`).

## Requirements

| Dependency | Role |
|------------|------|
| **Wolfram MCP** | Entity resolution, maps, timelines, conflict/event graphs |
| **skills** toolset | `skill_view` for worker brief, recipes, and lesson references |
| **`ailc-history`** | Required when running **`ailc-history-lesson`** (shared recipes/grounding) |
| **made-to-stick** (optional) | Sticky storytelling technique (SUCCESS); skills still work without it |

If Wolfram is unavailable, skills fall back to careful prose and do not invent map image URLs.

## Install (Hermes)

Register this repo as a [Hermes skills tap](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills#publishing-a-custom-skill-tap), then install. Use the `owner/repo` form — not a full GitHub URL.

```bash
hermes skills tap add warmersun/ailc-history-skill
hermes skills install warmersun/ailc-history-skill/skills/ailc-history --category ailc
hermes skills install warmersun/ailc-history-skill/skills/ailc-history-lesson --category ailc
```

After that, `/ailc-history` and `/ailc-history-lesson` are available in chat. To pick up upstream changes later:

```bash
hermes skills update ailc-history
hermes skills update ailc-history-lesson
```

`tap add` only registers the source; `install` is what copies `SKILL.md` and `references/` into `~/.hermes/skills/`. Prefer the install identifiers above over `hermes skills search` — custom taps are easy to miss in search (Hermes may skip GitHub when its index is available, and `--source github` can time out while scanning the large default taps).

Ensure Wolfram MCP is configured (`WolframContext`, `WolframAlpha`, `WolframLanguageEvaluator` or equivalent).

## Usage

### Interactive (`/ailc-history`)

- “Roman Empire around 100 AD”
- “Timeline of the Thirty Years’ War”
- “What was happening in the world when the Ming dynasty fell?”
- “Map of the Ottoman Empire in 1520”

Typical turn shape:

1. `delegate_task` fan-out (each child context mandates `skill_view` of worker-brief + wolfram-recipes)  
2. High-level framing in plain language after the handle returns  
3. Later synthesis in the reply format: headed sections, full sentences that gloss terms, short tables when useful, one strong visual, a next hook  

### Prepared lesson (`/ailc-history-lesson`)

- “Prepare a history lesson on Mohács 1526 for a high-schooler, about two hours”
- “Write a chaptered self-study pack on the Roman Empire around 100 AD”
- “Lesson package: French Revolution for undergrad survey”

Output lives under `./output/<slug>/` with `index.md` as the entry point.

## Entity types (Wolfram)

Canonical list and evaluator rules live in [`skills/ailc-history/references/wolfram-recipes.md`](skills/ailc-history/references/wolfram-recipes.md). Names are always resolved with free-form / typed interpreters — never hand-written canonical entity IDs.

## Design principles

- **Learner-facing only** — no pedagogy jargon or orchestration labels in learner prose  
- **Compute over guess** — maps and timelines from Wolfram when geography or chronology matter  
- **DRY references** — WL rules only in `ailc-history` wolfram-recipes; lesson skill reuses them  
- **Curate** — few strong artifacts beat long dumps  
- **Label reconstructions** — AI images are modern impressions, never substitutes for dated maps  
- **Interactive vs offline** — chat tutor optimizes for speed; lesson author optimizes for complete navigable files  

## License

[MIT](LICENSE) — Copyright (c) 2026 Sic
