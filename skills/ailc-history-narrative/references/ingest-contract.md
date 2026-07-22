# Ingest Contract — History Narrative from Lesson Pack

This skill **never researches a history topic from scratch**. It transforms an existing **`ailc-history-lesson`** package into polished narrative non-fiction.

## Required input

A lesson folder produced by `/ailc-history-lesson`, typically:

```text
./output/<lesson-slug>/
  index.md
  question-ledger.md
  learner-profile.md      # preferred; may be thin
  chapters/NN-*.md
  assets/                 # preferred
  sources.md
```

The user may pass an absolute or relative path (e.g. `hermes/output/first-temple-destruction-lesson`). Resolve it before writing.

## Refuse if

- No path is given **and** no obvious `./output/*-lesson/` exists for the named topic.
- `index.md` is missing.
- There are no chapter files and no substantial body in the index alone.

Tell the user to run `/ailc-history-lesson` first (or point at the pack path). Do **not** invent a research corpus.

## What to read (mandatory)

1. `index.md` — title, abstract, goals, TOC  
2. All `chapters/*.md`  
3. `question-ledger.md` — for priorities and unresolved tensions (do **not** paste ledger structure into the narrative)  
4. `learner-profile.md` — level, constraints, time budget (shape vocabulary only; **never** name the audience in prose)  
5. `sources.md` — evidence boundaries  
6. Scan chapters/ledger for `<!-- STORY-SEED -->` markers  

## What you may add

- Story hunting (new scenes/anecdotes that **illustrate** claims already supported by the pack)  
- Light verification of story details (dates, names) against the pack + careful web lookup  
- New story-illustration images if the spine needs a scene the pack lacks  

## What you must not do

- Re-run the full world-context / question-ledger research loop  
- Contradict the pack’s attested facts without explicit note and evidence  
- Use Wikipedia as encyclopedia source (Grokipedia / pack sources / primary preferred)  
- Invent primary-source quotations  

## Output location

```text
./output/<lesson-slug>-narrative/
  index.md
  chapters/NN-*.md          # chaptered (default for multi-beat topics)
  OR <slug>-narrative.md    # single article when the arc is one movement
  assets/                   # copies from lesson and/or new story art
  sources.md
  narrative-plan.md         # optional author-only: Core Message, spine (not required for reader)
```

Prefer **copying** needed map/timeline files from the lesson `assets/` into the narrative `assets/` so the narrative folder is self-contained. If copy is awkward, relative links back to the lesson pack are acceptable — document the dependency in `sources.md`.
