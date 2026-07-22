# History Question Ledger — Lesson Authoring

The question ledger is the **map of the lesson**. Chapters exist to answer high-value questions in a teachable order — not to dump chronology.

Write the living ledger to `./output/<slug>/question-ledger.md`.

Use four levels:

| Level | Role |
|-------|------|
| **Primary** | 8–15 questions a smart learner at the target level needs answered |
| **Secondary** | 4–8 per primary when the topic is non-trivial; sharpen mechanism, evidence, comparison |
| **Rabbit holes** | Triggered by contradictions, myths, missing maps, contested historiography |
| **Unresolved** | Still open at ship time, with reason and what would close them |

---

## How to generate high-value history primaries

**Good questions:**

- Would change how the learner *pictures* the past if answered differently  
- Force **when / where / who / why it mattered** with evidence  
- Expose myth vs record or national-memory distortions  
- Connect the local topic to **world context**  
- Are answerable with maps, sources, or careful secondary synthesis  

**Bad questions:**

- Restate the topic title (“What was the Roman Empire?”) without a teaching angle  
- So broad they need a multi-volume history  
- Pure trivia with no implication  
- “List every emperor” dumps  

**Prompts:**

- What would a skeptical smart beginner need before they could explain this to a friend?  
- What assumption in the popular story, if false, flips the picture?  
- What was scarce (money, legitimacy, horses, grain, legitimacy of succession)?  
- Who disagrees, and what is their strongest point?  
- What was happening on the other side of the world the same year?

---

## Seed primary lanes (history)

Cover all that apply; drop lanes that truly do not fit.

1. **Framing** — What is this topic, and what neighboring topics are we *not* covering?  
2. **When** — What year/range; which historical periods apply; any periodization debate?  
3. **Where at scale** — Which continent/world theaters matter; what does a dated map show?  
4. **Powers** — Who held power; what kind of state/society was this?  
5. **Conflict** — What wars/revolutions involved these powers; how do actors cluster?  
6. **Focus deep-dive** — What must the learner understand about the named person/event/polity?  
7. **Human-scale life** — What sticky story makes institutions feel real?  
8. **Economy / environment / tech** — What material constraints drove decisions?  
9. **Belief, art, culture** — What should the learner see or hear from the age?  
10. **Primary voices** — Which short sources still speak (and what are their biases)?  
11. **Afterlife & memory** — How did later ages remember, use, or mythologize this?  
12. **Contested claims** — Where do serious accounts diverge?  
13. **Learner goals** — Which 3–5 takeaways must stick for *this* profile?  
14. **Comparisons** — What parallel case (rival empire, earlier crisis, later echo) clarifies?  

---

## Secondary question craft

For each primary, add secondaries that:

- Demand a **mechanism** (“How did logistics actually work?”)  
- Demand a **comparison** (“How did Parthia differ from Rome as a peer?”)  
- Demand **evidence** (“What source says that? How close is it to the event?”)  
- Demand **boundaries** (“When does this explanation stop working?”)  
- Are **falsifiable** or at least checkable against maps/sources  

Example under a primary *“Who were Rome’s peers c. 100 AD?”*:

- Which powers appear on a Europe–Near East continent map in 100?  
- Was Parthia a “barbarian fringe” or a peer empire — and by what measure?  
- What conflicts were active in the decade before/after 100?  
- Which borders are well attested vs poorly mapped in the Knowledgebase?  
- How would a traveler’s itinerary cross rival jurisdictions?

---

## Rabbit holes (history-specific)

Open a rabbit hole when research shows:

| Trigger | Example chase |
|---------|----------------|
| Popular myth | “Nero fiddled while Rome burned” → what sources actually say |
| Map failure | No polygon for year Y → nearby year + disclosure |
| Name collision | Multiple “Louis II” / dual dating systems |
| National narrative | Textbook hero story vs multi-sided campaign history |
| Anachronism risk | Using modern nation names for medieval polities |
| Source bias | Victor’s chronicle only → find a counter-voice or label the gap |

**Chase depth-first** 2–4 layers, then close when the lead no longer changes what the chapter must teach — document peripheral leads as Unresolved or “further reading.”

---

## Answering discipline (for ledger notes)

In `question-ledger.md`, answers may be terse and labeled (Resolved / Partial / Unresolved).

When the **same** question is taught in a chapter, rewrite as clean teaching prose:

- Answer directly first  
- Then evidence (map, source, consensus secondary)  
- Then uncertainty  
- Then why it matters for the learner  

Under chapter headings use the italic guiding-question blockquote — never literal **Question:** / **Answer:** headings.

```md
## Parthia as peer

> ❓ _Was the Iranian empire to Rome’s east a fringe nuisance or a structural rival?_
```

---

## Chapter plan section (inside the ledger)

After the ledger stabilizes enough to design, add:

```md
## Chapter plan

| # | File | Owns primaries | Opening scene | Required visual | Forward hook |
|---|------|----------------|---------------|-----------------|--------------|
| 01 | 01-… | P1, P13 | … | … | … |
```

Update this table if research forces a restructure **before** mass chapter rewrites.

---

## User feedback on shallow questions

If the user says questions are shallow, “ask better questions,” or the lesson feels thin:

1. Produce a **v2 ledger** with higher secondary/rabbit-hole density  
2. Add a short revision note at the top of `question-ledger.md`  
3. Patch the affected chapters — do not only promise better next time  

---

## Ledger file skeleton

```md
# Question ledger — <topic>

**Slug:** …
**Level:** …
**Status:** draft | saturated
**Last research pass:** YYYY-MM-DD

## Revision notes
- …

## Primary questions

### P1 — …
- **Status:** Resolved | Partial | Unresolved
- **Answer:** …
- **Evidence:** …
- **Secondaries:**
  - S1.1 …
  - S1.2 …

## Rabbit holes
- R1 …

## Unresolved
- U1 … — reason …

## Chapter plan
| # | File | …
```
