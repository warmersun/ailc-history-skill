# AILC History

**AI Learning Companion — History**: agent skills for history learning grounded in the [Wolfram Knowledgebase](https://www.wolfram.com/knowledgebase/) via Wolfram MCP.

This package ships **three skills**:

| Skill | Mode | Use when… |
|-------|------|-----------|
| **`ailc-history`** | Interactive chat tutor | Live exploration, maps on demand, Socratic Q&A |
| **`ailc-history-lesson`** | Research + self-study pack | Question ledger, maps, chaptered teaching files on disk |
| **`ailc-history-narrative`** | Polished non-fiction | Essay / mini-book **from an existing lesson pack** only |

```text
Topic ──► ailc-history-lesson (broad research pack)
              │
              └──► ailc-history-narrative (narrow sticky prose)
```

Interactive tutoring is separate (`ailc-history`). Lesson + narrative mirror the double-diamond pattern of research → writing.

## What they do

### `ailc-history` (interactive)

- Places a topic in **world context** (when, where at scale, who held power, who was at war)
- Builds **historical maps and timelines** from Wolfram
- Surfaces **primary sources**, period art/coins, and carefully labeled AI reconstructions
- Keeps the chat **fast** via parallel Hermes sub-agents
- Stays honest about missing data, contested historiography, and myth vs record

### `ailc-history-lesson` (research / lesson package)

- Profiles the learner; builds a **question ledger**
- Writes a navigable pack under `./output/<slug>/` (TOC, chapters, assets, sources)
- **Fans out subagents** for maps, timelines, and story images
- Marks vivid moments with `<!-- STORY-SEED -->` for the narrative skill
- Teaching-oriented: goals, guiding questions, maps — **not** magazine non-fiction

### `ailc-history-narrative` (polished prose)

- **Ingests only** an existing lesson pack — never researches a bare topic from scratch
- Story hunting + Core Message + single narrative arc
- Scene-first openings, SUCCESS / Zinsser craft, dense chaptered or single-article output
- Reuses lesson maps; optional new story illustrations
- Output: `./output/<slug>-narrative/`

## Repository layout

```text
.
└── skills/
    ├── ailc-history/
    │   ├── SKILL.md
    │   └── references/          # Wolfram recipes, grounding, pedagogy, …
    ├── ailc-history-lesson/
    │   ├── SKILL.md
    │   └── references/          # intake, ledger, worker-brief, visuals, …
    └── ailc-history-narrative/
        ├── SKILL.md
        └── references/          # ingest contract, voice, story-hunting, craft
```

Lesson and narrative skills **depend on** `ailc-history` references where needed (not duplicated). Narrative **depends on** a lesson pack on disk.

## Requirements

| Dependency | Role |
|------------|------|
| **Wolfram MCP** | Maps, timelines, entities (tutor + lesson; narrative only if new art needs history tooling) |
| **skills** toolset | `skill_view` for references |
| **`ailc-history`** | Shared recipes/grounding for tutor + lesson |
| **Lesson pack path** | Required for **`ailc-history-narrative`** |
| **made-to-stick** (optional) | SUCCESS; narrative also ships a local SUCCESS note |

If Wolfram is unavailable, skills fall back to careful prose and do not invent map image URLs.

## Install (Hermes)

```bash
hermes skills tap add warmersun/ailc-history-skill
hermes skills install warmersun/ailc-history-skill/skills/ailc-history --category ailc
hermes skills install warmersun/ailc-history-skill/skills/ailc-history-lesson --category ailc
hermes skills install warmersun/ailc-history-skill/skills/ailc-history-narrative --category ailc
```

Updates:

```bash
hermes skills update ailc-history
hermes skills update ailc-history-lesson
hermes skills update ailc-history-narrative
```

Use the `owner/repo` form — not a full GitHub URL. Ensure Wolfram MCP is configured for map-heavy work.

## Usage

### Interactive (`/ailc-history`)

- “Roman Empire around 100 AD”
- “What was happening in the world when the Ming dynasty fell?”

### Lesson pack (`/ailc-history-lesson`)

- “Prepare a history lesson on Mohács 1526 for a high-schooler, about two hours”
- “Self-study pack: Destruction of the First Temple”

### Narrative (`/ailc-history-narrative`)

- “Write a narrative mini-book from `./output/first-temple-destruction-lesson/`”
- “Polish that lesson pack into a sticky longform essay”
- Requires the lesson folder first — will not invent research from a bare topic

## Entity types (Wolfram)

Canonical list and evaluator rules: [`skills/ailc-history/references/wolfram-recipes.md`](skills/ailc-history/references/wolfram-recipes.md). Always resolve names with free-form interpreters — never hand-written entity IDs.

## Design principles

- **Learner-facing only** in teaching/narrative prose — no pedagogy brand names  
- **Compute over guess** — maps and timelines from Wolfram when geography or chronology matter  
- **DRY references** — WL rules only in `ailc-history`  
- **Double diamond** — lesson broad; narrative narrow  
- **Label reconstructions** — AI images are modern impressions, never substitutes for dated maps  
- **Honest historiography** — myth vs record, contested claims  

## License

[MIT](LICENSE) — Copyright (c) 2026 Sic
