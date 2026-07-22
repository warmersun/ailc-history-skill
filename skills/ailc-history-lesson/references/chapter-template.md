# Chapter Template — History Lesson Files

Use this shape for every file under `./output/<slug>/chapters/`.

Adapt length to level and time budget. Keep **navigation**, **at least one visual**, and **learner-facing voice** mandatory.

---

## Filename

`NN-<short-slug>.md` — clean sequential numbering (`01-`, `02-`, …). No `00a-` style prefixes.

---

## Full template

```md
# <Topic-facing chapter title>

**Navigation:** [← Previous](0N-prev-slug.md) · [Table of contents](../index.md) · [Next →](0N-next-slug.md)

<!-- First chapter: omit Previous. Last chapter: Next may point to ../sources.md or be omitted. -->

---

## <Opening section — scene or core idea>

> ❓ _<One guiding question for this section>_

<1–3 paragraphs: concrete scene or clear core claim. Define any new term on first use.
Prefer a human-scale moment (named person, place, year) over an abstract thesis.>

![<short alt text>](../assets/<filename>.png)

*<Caption: what / when / why this image belongs here. If AI: “Modern reconstruction — details approximate.”>*

---

## <Second major beat>

> ❓ _<Guiding question>_

<Full-sentence teaching prose. Short table OK for terms, dates, or compare/contrast.>

| Term | Plain meaning |
|------|----------------|
| **…** | … |

<!-- Additional visuals if the argument needs them — do not decorate. -->

---

## <Optional third beat>

…

---

## Takeaway

<2–4 sentences: what should stick from this chapter alone.>

**Next:** <One sentence forward hook naming the tension or question the following chapter opens — plain language, not “In the next chapter we will discuss…” bureaucracy if you can avoid it.>

---

**Navigation:** [← Previous](0N-prev-slug.md) · [Table of contents](../index.md) · [Next →](0N-next-slug.md)
```

---

## Navigation rules

| Position | Previous | TOC | Next |
|----------|----------|-----|------|
| First chapter | omit or `—` | `../index.md` | `02-…md` |
| Middle | prior file | `../index.md` | following file |
| Last | prior file | `../index.md` | optional `../sources.md` or omit |

Use **relative paths only** so the pack moves as a folder.

Place the nav block **at top and bottom** of every chapter.

---

## Voice checklist (before saving)

- [ ] No skill/process jargon (no SUCCESS, context pass, ledger, Wolfram recipe names as headings)  
- [ ] No “as instructed” / “per the skill” / “I generated a map”  
- [ ] Terms glossed on first use  
- [ ] Myth vs record labeled when a popular story appears  
- [ ] Quotes attributed or omitted (never invented)  
- [ ] AI images labeled modern reconstruction  
- [ ] Map captions disclose actual year/entity if fallback  

---

## Visual placement

- Prefer **immediately after** the paragraph that needs the spatial or chronological argument  
- One strong visual beats three weak ones  
- Story illustrations sit with the story paragraphs, not in a dumping ground at the end  
- Asset paths: `../assets/<file>` from `chapters/`  

Details: `visual-assets.md`. Shared honesty rules: `ailc-history` → `references/grounding.md`.

---

## Optional end-of-chapter self-check

Only if learner goals include exam/self-test:

```md
## Check yourself

1. …
2. …
3. …
```

Keep questions answerable from **this chapter + earlier chapters**, not from hidden author notes.

---

## What not to put in chapter files

- YAML frontmatter (keep on `index.md` only if needed)  
- Full bibliography (use `sources.md`; short inline attribution is fine)  
- Question-ledger status tables  
- Raw Wolfram code or tool transcripts  
- Uncaptioned images  
