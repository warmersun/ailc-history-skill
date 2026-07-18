# Learner-facing reply format

How teaching turns should look. Validated in live tutoring (Ivan IV / Kazan, 2026-07): the learner liked **full sentences that define terms + headings + short tables + Wolfram visuals + real art photos when asked**.

This complements the voice rules in `SKILL.md`: structure the reply for reading, but never surface pedagogy jargon, SUCCESS labels, orchestration roles, or tool/recipe names as section titles.

## Default shape

**Structure + full sentences** — not a slide deck of bullets, and not a wall of unbroken paragraphs.

| Do | Don’t |
|----|--------|
| **Headings and subheadings** for every major beat (topic-facing titles) | Bullet-only dumps with no narrative |
| **Full sentences** that teach and **define terms on first use** | Jargon dropped bare without a gloss |
| **Short tables / short lists** when they clarify | Overlong tables that replace teaching |
| **Keep Wolfram visuals** (maps, timelines as markdown image links) | Prose-only turns when geography or chronology is the point |
| **Define unfamiliar terms on first use** in plain language | Assume undergrad vocabulary |
| Plain status (“I’m pulling a wider map…”) | Meta labels (“SUCCESS-shaped”, “context pass”, “sub-agent 2”) |

## Approved skeleton

1. **Title** — topic-facing, not a process label  
2. **Short core idea** (1–3 sentences)  
3. **Headed sections** in full sentences  
4. **One or two short tables** (terms, dates, compare/contrast) when useful  
5. **Wolfram map/timeline images** embedded where geography or chronology matters  
6. **Next hooks** (short list OK)

Light turns (a quick clarification) can compress this; multi-part teaching turns should not skip structure or visuals when those are the point.

## Balance pitfall

Swinging from “all bullets” to “all paragraphs, no structure, no visuals” both fail.

If the learner corrects format mid-thread, **rebuild the last teaching point** in the corrected shape immediately — do not only promise to do better next time.

## Term gloss (pattern)

Gloss on first use, inline or in a tiny terms table:

- **boyar** — high hereditary noble near the top of Muscovite/Russian society  
- **khanate** — state ruled by a khan (often a Tatar successor of the Horde)  
- **oprichnina** — Ivan IV’s personal domain plus black-clad enforcers used as terror police  

Reuse the same pattern for any unfamiliar polity, office, or institution in the era under discussion.

## Period photos / exterior art (not Wolfram plots)

When the learner asks for **pictures of a standing building, painting, coin, etc.**:

1. Prefer **real photographs or museum images**, not AI illustration.  
2. **Do not invent** Wikimedia (or other CDN) paths. Guessed `upload.wikimedia.org/.../hash/File.jpg` URLs often **404**.  
3. Resolve a real URL first (Commons API: exact `File:` title → `imageinfo.url`, or another verified source).  
4. Deliver via whatever media path the host supports (Hermes desktop: download/resize locally, then `MEDIA:/absolute/path/to/file.jpg`). Prefer a durable local copy when remote embeds are flaky.  
5. If an embed breaks, re-resolve and re-send — do not keep pasting the same broken URL.  
6. Caption: what / when / why it belongs in this turn.

AI illustration remains allowed for atmosphere when period imagery is thin — label it as a **modern reconstruction**, and never use it instead of a Wolfram map or timeline.

## Keep map craft out of the prose layer

Reply format is about how you *write*. Drawing rules live in the recipes:

- Markers, image size, entity failure: `references/wolfram-recipes.md`  
- Rivers as the teaching argument: `references/rivers-natural-earth.md`  

Split heavy Wolfram work into separate evaluator calls when bundles time out; tell the learner in plain language that you are looking things up, without naming tools.
