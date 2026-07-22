# Lesson Structure — Chapter Arcs for History Packages

How to turn a saturated history question ledger into a navigable multi-chapter lesson.

---

## Design principles

1. **World before tunnel vision** — early chapter(s) pin when/where/powers; later chapters zoom.  
2. **One job per chapter** — each file owns 1–3 primaries; avoid repeating the same synthesis.  
3. **Scene → concept → implication** — open concrete; define institutions; land why it matters.  
4. **Visual as argument** — maps/timelines teach geography and time; they are not decoration.  
5. **Story as memory hook** — one strong human-scale thread beats five name lists.  
6. **Forward hooks** — every chapter ends by naming the tension the next chapter resolves.  
7. **Profile fit** — chapter count and density follow time budget and level (intake).  

---

## Default extensive arc (4–8 chapters)

| # | Role | Typical contents | Typical visual |
|---|------|------------------|----------------|
| **01** | Hook & framing | Concrete scene; what the lesson covers / skips; goals in plain language | Story illustration or period object |
| **02** | World context | Year/range; continent or world geopolitics; big powers named | Continent/world historical map |
| **03** | Focus zoom | The polity, person, or event in place | Historical country map or person timeline |
| **04** | Conflict / crisis | Wars, alliances, clusters, stakes | War map and/or conflict-community summary |
| **05** | Human-scale story | One sticky narrative that embodies institutions | Story scene (period art or modern reconstruction) |
| **06** | Culture, belief, material life | Art, coins, cities, religion, daily constraints | Period art / coin / architecture photo |
| **07** | Voices & afterlife | Short primary quote; how later ages remembered this | Manuscript/photo or second reconstruction |
| **08** | Synthesis | Takeaways matched to goals; open questions; “where to go next” (optional `ailc-history` chat) | Combined timeline or recap map |

**Compress** to 4–5 when time budget is ~1–2 hours: merge 05–07; keep 01, 02, 03–04, synthesis.

**Expand** beyond 8 only when the user asks for course-length depth or multiple distinct theaters (then consider part divisions in the TOC).

---

## Fast-mode shapes

| Request | Shape |
|---------|--------|
| Outline only | `index.md` + chapter plan in ledger; stub chapters optional |
| Single deep dive | `01` framing+context, `02` focus, `03` story+sources |
| Add-on chapter | One new `NN-*.md`; renumber only if order breaks; update TOC |

---

## Level adaptations

| Level | Adjustments |
|-------|-------------|
| **Middle school** | Shorter chapters; more gloss tables; gentler violence; stronger story spine; fewer primaries in ledger |
| **High school** | Default arc; exam-friendly date table in synthesis if goals say so |
| **Undergrad survey** | Stronger historiography honesty; primary-source chapter; comparison secondary |
| **Adult curiosity** | Rich stories + world context; light on exam apparatus |
| **Specialist refresh** | Can compress 01–02; spend pages on contested claims and sources |

---

## Ordering rules

- **Respect user-specified order** when they name chapter sequence or starting concept.  
- Otherwise prefer: frame → world map → focus → conflict → story → culture/sources → synthesis.  
- Do **not** open with a pure abstract thesis if a concrete scene can carry the same idea.  
- Military topics still get a non-war beat (logistics, civilians, art, diplomacy) somewhere in the pack.  

---

## What each chapter owes the pack

- Advances **learner goals** from the profile  
- Answers its owned ledger primaries (or explicitly defers with a link to another chapter)  
- Introduces **no unexplained jargon**  
- Contains **≥1 inline visual** (see `visual-assets.md`)  
- Ends with a **forward hook** (except final synthesis, which may hook to further study)  
- Links **Previous / TOC / Next**  

---

## TOC patterns in `index.md`

```md
## Table of contents

1. [A courier on the edge of empire](chapters/01-courier-hook.md)
2. [The world in 100 AD](chapters/02-world-in-100.md)
3. [Rome under Trajan](chapters/03-rome-trajan.md)
…
```

Optional grouping:

```md
### Part I — Setting the stage
1. …
2. …

### Part II — Crisis and memory
3. …
```

---

## Anti-patterns

| Avoid | Prefer |
|-------|--------|
| Chronological annal dump (year1, year2, …) | Causal and spatial arcs with selected dates |
| All maps in an appendix | Maps **inline** where the argument needs them |
| Chapter titles like “Context pass” / “SUCCESS story” | Topic-facing titles |
| Repeating the full world survey in every chapter | One strong context chapter + brief back-references |
| Ending chapters with “In conclusion, …” filler | One punchy takeaway + forward hook |
| Eight thin stubs | Fewer dense chapters that actually teach |

---

## Optional extras (only if goals demand)

- **Check questions** at chapter ends (self-study), clearly separated from narrative  
- **Glossary** chapter or section for dense institutional vocabulary  
- **Timeline appendix** as a final file if dates are an explicit goal  
- **Further paths** pointing to interactive exploration with `ailc-history`  

Do not add graded rubrics or “will this be on the test?” framing unless the user asked for exam prep.
