# Visual Assets — Static History Lessons

How to produce, save, caption, and embed maps, timelines, and story images in the lesson package.

**Authority for historical visual honesty and recipe choice:** always load **`ailc-history`** references:

- `references/grounding.md` — matrix, art, AI illustration, Grokipedia rule  
- `references/wolfram-recipes.md` — maps, timelines, entities, failure loops  
- `references/rivers-natural-earth.md` — when rivers are the teaching argument  
- `references/worker-brief.md` — if delegating thin visual jobs  

This file only covers **on-disk packaging** for prepared lessons.

---

## Directory convention

```text
./output/<slug>/assets/
  02-world-100ad.png
  03-roman-empire-100.png
  05-story-trajan-road-reconstruction.png
  …
```

Naming: prefer `<chapterNN>-<short-role>.<ext>` so authors can find assets quickly.

Supported: `.png`, `.jpg`, `.jpeg`, `.webp`, `.svg` (if the host produced SVG).

---

## What to generate (priority)

| Need | Prefer | Source |
|------|--------|--------|
| World/continent geopolitics | Dated historical map | Wolfram continent/world recipe |
| One empire/kingdom | Historical country map | Wolfram country-in-year |
| War / battle theater | War map | Wolfram war map |
| Sequence of eras/lives | Timeline | Wolfram timeline recipes |
| Standing building / coin / painting | Real photo or museum image | Resolved URL → download to `assets/` |
| Story moment with no good period image | AI reconstruction | Image gen; label in caption |

**MUST NOT** use AI or decorative art **instead of** a Wolfram map/timeline when geography or chronology is the teaching point.

**MUST NOT** invent Wikimedia or CDN paths. Resolve a real URL, then save a local copy under `assets/` when possible so the lesson works offline.

---

## Embedding in chapters

From `chapters/0N-….md`:

```md
![Europe and the Near East, political geography c. 100 AD](../assets/02-world-100ad.png)

*Continent-scale view c. 100 AD (Wolfram historical map). Major powers labeled in the surrounding text. If the map year differs from the lesson year, say so here.*
```

Story / AI example:

```md
![Modern reconstruction: a Roman courier on a paved road in a provincial landscape](../assets/05-story-courier-reconstruction.png)

*Modern AI reconstruction of a courier on a Roman road, Trajanic period (c. 100 AD) — clothing, kit, and landscape are approximate, not a period photograph.*
```

Rules:

- Meaningful **alt text** (not “image1”)  
- Caption states **what / when / why**  
- Disclose **fallback year or entity** when Wolfram returned a neighbor  
- AI: always **modern reconstruction** language  

---

## Saving workflow (author)

1. Run Wolfram recipe or image find/generate (yourself or thin worker).  
2. Write binary under `./output/<slug>/assets/` with a stable filename.  
3. Embed relative path in the chapter.  
4. Log origin in `sources.md` (entity free-text, year, recipe kind, or image URL / “AI reconstruction”).  

If the host returns a remote image URL only, **prefer downloading** to `assets/` for portability. If download is impossible, embed the verified remote URL and note dependency in `sources.md`.

---

## Minimum visual bar

| Mode | Bar |
|------|-----|
| **Every chapter** | ≥1 inline visual **or** an explicit short note that no reliable map/image exists and prose carries the load |
| **Extensive pack** | ≥1 world/continent context visual early; ≥1 focus map or timeline; ≥1 story illustration somewhere in the pack |
| **Story used** | That story **MUST** be illustrated (period/photo or labeled reconstruction) |

---

## Worker fan-out (optional)

When using subagents, keep goals thin (ailc-history worker-brief). Example context preamble:

```text
First tool calls: skill_view("ailc-history", "references/worker-brief.md"),
then skill_view("ailc-history", "references/wolfram-recipes.md").
Follow the worker brief. Return visuals + terse fact bullets only (≤3 deliverables).
Do not teach the learner; do not call delegate_task.
Save or return image paths suitable for embedding in a static lesson.
```

The **lesson author** still writes chapter prose and final `assets/` placement.

---

## Encyclopedia and art search

- Secondary encyclopedia: **Grokipedia only** — not Wikipedia (`grounding.md`).  
- Period art/coins: museum or verified hosts; caption medium and approximate date.  
- Honesty on missing polygons, contested borders, and mythic scenes — never fake certainty in a caption.  

---

## `sources.md` visual section (suggested)

```md
## Visuals

| File | Kind | Origin | Year / entity notes |
|------|------|--------|---------------------|
| assets/02-world-100ad.png | Wolfram continent map | Historical continent map recipe | Free-text: Europe, 100 AD |
| assets/05-story-….png | AI reconstruction | image_gen | Prompt summary: … |
```
