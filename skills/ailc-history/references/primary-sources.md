# Primary sources and letter deep-dives (parent)

**Load this file** whenever the learner asks whether a text exists, wants a letter/report **section walk**, manuscript status, or verified Latin/Greek quotes. It is part of `ailc-history` even if `SKILL.md` / `pedagogy.md` cross-links lag. Workers stay on maps/terse facts; the **parent** owns source teaching.

## Learner question shapes

| Ask | Teach |
|-----|--------|
| “Do we have those letters?” | Manuscript reality: witnesses, critical edition, authorial vs intermediary report — not a yes/no fairy tale |
| “Walk the letter” / section by section | Block table (order of sense + edition pages) → tactics / demand / warning beats → short Latin + plain gloss |
| Route + letters together | Keep geography from Wolfram maps; keep **text claims** tied to named edition |

## Honesty contract (MUST)

- Do **not** invent continuous primary text or long monologues and call them the source.
- Prefer **short verified quotations** (from a critical edition or a scholarly article that prints the Latin) + plain-language gloss.
- Label layers: **authorial letter** vs **report about** the traveler (e.g. Riccardus on Julian) vs **later chronicle echo**.
- If only a paraphrase/summary is available, say so (“scholars summarize this block as…”).
- Name the edition: editor, year, page range when known (e.g. Dörrie 1956, pp. 165–182).

## Retrieval path (parent)

Order of attack when Wolfram has little text:

1. **Grokipedia** for orientation (`site:grokipedia.com`) — not Wikipedia (see `grounding.md`).
2. **Open academic PDFs** — institutional repos (e.g. MTMT / `real.mtak.hu`), university pages. `curl` → `pdftotext` (or equivalent) and quote from the extract.
3. Scholarly apparatus: footnotes often print the **exact Latin** even when the full letter is paywalled.
4. Critical editions (library/OL): record title even if fulltext is offline so the learner has a cite path.
5. Skip paywall/Cloudflare walls without looping; teach with what you verified.

Do **not** treat a failed `web_extract` backend as “no sources exist” — retry with direct PDF/HTTP. Failed page-extract is an environment/tooling issue, not proof that sources are missing.

## Section-walk template

1. **What the text is** — title, author, date, addressee/audience.
2. **Manuscript / edition map** — 2–4 rows: witness or editor → role.
3. **Block table** — sense-order sections with edition page anchors when known.
4. **Walk 4–8 beats** — each: heading, what it does rhetorically, 0–2 short quotes, plain gloss.
5. **Rhetoric spine** (optional one table) — how the piece persuades (presence → method → named threat → urgency).
6. **Bridge** — what happened after the text (without stealing the next deep dive).
7. **Next hooks** — 3–4 doors.

Full-sentence teaching + term glosses still follow `reply-format.md`. One map/timeline **SHOULD** still appear when geography or chronology is in play; pure manuscript turns may lean on tables + quotes.

## Worked corpus: Friar Julian (1230s)

Condensed bank from tutoring use — verify against Dörrie/Hautala before asserting new claims.

| Text | Nature | When | Edition anchor |
|------|--------|------|----------------|
| Riccardus *Relatio* | Report **about** Julian’s first eastern journey | Finished early 1236 (return 27 Dec 1235) | Dörrie 1956, ~pp. 151–161 |
| Julian *Epistola de vita Tartarorum* | Julian’s own letter on Tartar ways / threat | Early 1238 | Dörrie 1956, ~pp. 165–182 |
| Later chronicles (e.g. Alberic) | Secondary echoes | After news spread | Not substitutes for the two reports |

**MSS for the 1238 letter (scholarly consensus line):** Ottobeuren copy (early print trail via Hormayr) → Innsbruck rediscovery → Vatican tradition (Lat. 4161 in apparatus). Three witnesses commonly cited.

**Critical edition home base:** Heinrich Dörrie, *Drei Texte zur Geschichte der Ungarn und Mongolen* (Göttingen, 1956). German annotated path: Göckenjan & Sweeney (1985). Modern synthesis useful for section map + printed Latin snippets: Roman Hautala, “Early Hungarian Information on the Beginning of the Western Campaign of Batu (1235–1242),” *Acta Orientalia ASH* 69.2 (2016) — open PDF often at `https://real.mtak.hu/38024/1/062.2016.69.2.5.pdf`.

**1238 letter sense-order (Hautala → Dörrie pages):**

| Block | ~pp. | Job |
|-------|------|-----|
| East in flames / cannot reach Volga Magyars | 165–168 | Discovery obsolete; refugees |
| Chinggis, Jochi, older wars | 169–172 | Empire with a past; Magyar war “14 years… 15th” (*expugno*) |
| Early Batu western moves | 172–173 | Front moving |
| Dispositions before eastern Rus’ | 173–174 | Staff picture; Ryazan eve (Julian leaves ~Sep–Oct 1237) |
| Parallel Caucasus | 175 | Multi-front machine |
| **Tactics** | **176–177** | How they win |
| **Yuri of Vladimir’s warning** | **177–179** | Hungary named |
| **Ultimatum preamble** | **179** | *Ego, Chayn…* |
| Failed Dominican follow-ons / Mordvins | 180–181 | Collateral proof |
| Eschatological coda | 181–182 | Pseudo-Methodius / Ishmaelites framing |

**Verified short Latin (quote only these or newly checked lines):**

- Envoy / open-ended war (Riccardus layer, Dörrie ~159):  
  `De terra sua exire proponit, pugnaturi cum omnibus, qui eis resistere voluerint, et vastaturi omnia regna quecumque poterunt subiugare.`
- Ultimatum preamble (Julian 1238, Dörrie ~179):  
  `Ego, Chayn, nuncius regis celestis, cui dedit potentiam super terram subicientes mihi se exaltare et deprimere adversantes.`
- Refugees willing to convert if they reach Christian Hungary (Dörrie ~180):  
  `Libenter fidem catholicam recepissent, [et] dum versus Ungariam christianam venissent.`
- “Giant-headed” peoples beyond Tartar land (Riccardus layer, Dörrie ~158–159) — ethnographic/envoy color; handle as reported speech, not zoology.

**Pitfalls**

- Do not collapse Riccardus and Julian into one “diary.”
- Sinor-style skepticism (Riccardus invention / no Magna Hungaria) is a **historiographical debate** — present as contested, not settled fact; mainstream still treats 1235 contact as real while reading Riccardus carefully (alliance vs long war tension).
- Trip dating wobbles (1234–35 vs 1235–36 start); firm handles: **27 Dec 1235** return, **early 1238** letter, Mohi **11 Apr 1241**.
- Academia.edu / Cloudflare often blocks automation — prefer MTMT/open PDFs and edition cites.

## Related

- Quote length and primary-first preference: `pedagogy.md`
- Encyclopedia rule + no invented quotes: `grounding.md`
- Learner prose shape: `reply-format.md`

## Maintenance note for skill authors

When curator `patch`/`edit` on `SKILL.md` / existing refs is available, keep one-line pointers from:

- `SKILL.md` → Stories and sources + References list  
- `pedagogy.md` → Sources (deep source turns)  
- `grounding.md` → encyclopedia fallback + letter quote honesty  

Until then, **linked_files** exposure of this path is enough if agents scan support files; the load rule at the top of this file is authoritative.
