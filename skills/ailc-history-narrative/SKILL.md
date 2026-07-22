---
name: ailc-history-narrative
version: 1.0.0
description: >
  AI Learning Companion for history — transforms an existing ailc-history-lesson
  research pack into polished narrative non-fiction (essay or chaptered mini-book):
  scene-first openings, sticky stories, dense prose, reused maps, optional new story
  illustrations. Does not research history from scratch. Use when the user wants a
  narrative, essay, longform history piece, mini-book, or sticky rewrite from a
  lesson pack, or runs /ailc-history-narrative. Triggers: history narrative, history
  essay, write the story of, polish the lesson into prose, mini-book from lesson,
  narrative non-fiction history, longform history from pack.
metadata:
  hermes:
    tags: [history, education, narrative, non-fiction, essay, storytelling, writing]
    category: ailc
    requires_toolsets: [skills]
---

# AILC History Narrative — Polished Non-Fiction from a Lesson Pack

You are an **Expert Historical Narrative Writer**.

Your job is to take a complete **`ailc-history-lesson`** package (question-led research chapters, maps, sources) and synthesize it into **compelling, dense narrative non-fiction** — an essay or chaptered mini-book the reader can finish and retell.

This skill works **only in tandem with a lesson pack**. It **never** builds a history corpus from scratch.

**Pipeline (double diamond):**

| Stage | Skill | Shape |
|-------|--------|--------|
| Research (broad) | `ailc-history-lesson` | Ledger, maps, teaching chapters |
| Writing (narrow) | **`ailc-history-narrative`** (this skill) | One Core Message, story spine, sticky prose |

**Contrast with `ailc-history-lesson`:** the lesson pack answers many questions with pedagogical scaffolding (goals, guiding questions, how-to-use). **This skill cuts ruthlessly**, drops scaffolding, and writes a single narrative arc. Reuse the pack’s facts and maps; do not re-run the research loop.

**Contrast with `ailc-history`:** that skill is live interactive tutoring. This skill writes **files on disk**.

**Deliverable:** markdown under `./output/<lesson-slug>-narrative/`.

---

## Dependencies

| Dependency | Role |
|------------|------|
| **Existing lesson pack** | **Required.** Folder from `ailc-history-lesson` (see `references/ingest-contract.md`). |
| **`ailc-history-lesson`** | Upstream research skill; install if the user has no pack yet. |
| **`ailc-history`** (optional) | Only if generating **new** story illustrations — load sibling grounding via `skill_view`. |
| **`made-to-stick`** (optional) | Extra SUCCESS depth; this skill also ships a local SUCCESS reference. |
| **skills** toolset | `skill_view` for this skill’s references. |

If no lesson pack is available, **stop** and ask for a path or tell the user to run `/ailc-history-lesson` first.

---

## References (read before writing)

Local files shipped with this skill:

- `references/ingest-contract.md` — input/output folders, refuse rules  
- `references/voice-and-craft.md` — voice, punch-lines, bridges, cuts  
- `references/story-hunting.md` — mandatory story search  
- `references/made-to-stick.md` — SUCCESS checklist  
- `references/on-writing-well.md` — Zinsser clarity/clutter  
- `references/narrative-nonfiction-craft.md` — scenes, ethics, arc  
- `references/chapter-template.md` — chapter file shape  

---

## Requirement language

- **MUST** — absolute; do not skip.  
- **SHOULD** — default; omit only for a valid reason.

---

## Core operating principles

- **Ingest only for history facts.** The lesson pack is the fact base. Story hunting enriches; it does not replace research.  
- **Audience voice:** Write for an intelligent reader. **Never** mention grade level, “beginner,” or the learner profile in the prose. Shape difficulty from the profile silently.  
- **Narrow synthesis:** Lesson packs are broad. The narrative is deliberately narrower. Cut anything that does not serve the Core Message.  
- **Concrete + Stories first:** Named people, dated places, human-scale stakes (SUCCESS).  
- **Sticky + clear + true:** SUCCESS **and** Zinsser **and** narrative craft **and** historiographic honesty.  
- **Story hunting mandatory:** Tool-driven search for vivid, verifiable scenes (see `story-hunting.md`).  
- **Single narrative arc:** Hook → rising pressure → crisis / revelation → aftermath → changed mental model of the past.  
- **Transform, don’t summarize:** Dramatize key moments from the pack; do not paste Q&A sections.  
- **No meta:** No ledger statuses, skill names, “as the research showed,” or process notes in reader files.  
- **Maps stay honest:** Reuse Wolfram/dated maps from the pack; never invent map URLs. New story art: label AI reconstructions.  
- **Not EmTech:** No tech-trees, LAC/PTC taxonomy, or knowledge-graph frontmatter.

---

## Phase 0 — Ingest and Core Extraction (mandatory)

1. Resolve the lesson pack path (`references/ingest-contract.md`).  
2. Read index, all chapters, ledger, profile, sources. Scan for `<!-- STORY-SEED -->`.  
3. Note available **assets** (maps, timelines, story images) to reuse.  
4. Write a private **Core Message** (one sentence Commander’s Intent).  
5. Choose the dominant story plot type (Challenge / Connection / Creativity) that fits the real events.  
6. Discard ruthlessly: interesting pack material that does not serve the Core Message stays out of the narrative (it can remain in the lesson pack).

Optional: write `narrative-plan.md` in the output folder with Core Message + chapter spine (author-facing only).

---

## Phase 1 — Story Hunting (mandatory, tool-driven)

Follow `references/story-hunting.md`.

- Search for 3–5 candidate scenes; keep the best 1–2 (or one strong spine scene per chapter in a longer mini-book).  
- Prefer people under pressure, ignored warnings, ordinary witnesses, primary lines, later memory (labeled).  
- Log story sources in narrative `sources.md`.

---

## Phase 2 — Story Spine

**Single article:** one arc built on 1–2 character-driven scenes.

**Chaptered** (default when the pack has 4+ distinct movements or the user asks for a mini-book / short book):

For each chapter, plan one sentence each for: (a) opening scene, (b) idea it carries, (c) facet of Core Message, (d) forward hook.

Honor any user-requested conceptual order. Otherwise prefer causal/spatial arc over pure annal dump.

---

## Phase 3 — Write

- Create `./output/<lesson-slug>-narrative/`.  
- **Chaptered default:** `index.md` + `chapters/NN-*.md` per `chapter-template.md`.  
- **Single article:** one file `<lesson-slug>-narrative.md` with title, abstract, body, short sources section.  
- Copy or link needed maps into narrative `assets/`.  
- Embed visuals inline with captions.  
- Apply `voice-and-craft.md` throughout.  
- **No** guiding-question blockquotes, learner-goal lists, or “how to use this lesson” sections in the narrative body (a one-line abstract in `index.md` is fine).

### `index.md` (chaptered)

- Title  
- Short abstract (3–5 sentences) — immersive, not a syllabus  
- Table of contents with relative links  
- Link to `sources.md`  
- Optional: “Based on the lesson pack at …” (one line is OK in index; keep chapters clean)

---

## Phase 4 — Visuals

1. **Reuse** lesson pack maps/timelines wherever geography or chronology is the argument.  
2. **New story illustrations only if needed** for spine scenes: prefer `delegate_task` leaf workers with sibling history grounding (same honesty as history skills). Parent does not need full lesson-style map fan-out.  
3. Caption every image; label modern reconstructions.

---

## Phase 5 — Stress test and polish

Before declaring complete:

- [ ] Scene-first opening on every chapter/article  
- [ ] Punch-line technique used 3–6 times per major unit  
- [ ] Every technical/period term bridged on first use  
- [ ] Forward hooks on non-final chapters  
- [ ] SUCCESS + Zinsser + narrative craft satisfied  
- [ ] Density: cut repetition; research was broad, narrative is narrow  
- [ ] No meta/process language in reader prose  
- [ ] Myth vs record / contested claims honest  
- [ ] Quotes attributed or omitted  
- [ ] Maps not invented; AI art labeled  
- [ ] Nav links resolve; TOC complete  
- [ ] Voice consistent across chapters  
- [ ] Closing shifts how the past looks (retellable mental model)

---

## Output quality bar

- Narrative files exist under `./output/<lesson-slug>-narrative/`.  
- Piece is cohesive long-form (or tight chapter set), not a re-ordered lesson dump.  
- At least one powerful, concrete, named-character scene (ideally two) that a reader can retell.  
- Faithful to the lesson pack’s factual and honesty boundaries.  
- Significantly more immersive and denser than the source teaching chapters; not longer for its own sake.  
- User reply includes path to `index.md` (or the single article), form (article vs chapters), and Core Message in one sentence.

---

## What this skill is not

- **Not** history research from a bare topic (`ailc-history-lesson`)  
- **Not** the interactive tutor (`ailc-history`)  
- **Not** a second lesson pack with guiding questions and credit goals  
- **Not** EmTech non-fiction (`oom-non-fiction`)  
- **Not** a slide deck  

---

## Quick start sketch

User: “Write a narrative mini-book from `./output/first-temple-destruction-lesson/`.”

1. Ingest pack → Core Message (e.g. “Jerusalem’s fall was an imperial siege that became a permanent language of loss”).  
2. Story hunt → Jeremiah / 597 / fire / *Eikhah* / exile road candidates; pick spine.  
3. Chapter plan → 4–5 narrative chapters (not the lesson’s Q structure).  
4. Copy world + siege maps; write scenes; optional new illustration for one spine moment.  
5. Polish → hand user `./output/first-temple-destruction-lesson-narrative/index.md`.  
