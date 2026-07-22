# Learner Intake Question Ledger — History Lesson

Use a four-level intake ledger:

- **Primary questions** — must resolve (or explicitly Unresolved) before extensive lesson writing  
- **Secondary questions** — deepen level fit, scope, constraints, assessment goals  
- **Rabbit holes** — contradictions (e.g. “exam next week” vs “multi-week curiosity deep-dive”)  
- **Unresolved** — remaining gaps with reasons and who can close them (learner, teacher, parent)

Write distilled answers into `./output/<slug>/learner-profile.md` and keep the full Q&A trail in `question-ledger.md` (intake section) or a short intake block at the top of the profile file.

---

## Seed primary questions

1. **What is the topic?**  
   (Person, polity, war, battle, period, place, theme, comparison — e.g. “Mohács 1526”, “Ming fall”, “Roman Empire c. 100 AD”.)

2. **Who is the learner and what level?**  
   (Middle school / high school / undergrad survey / undergrad seminar / adult curiosity / teacher prep / specialist refresh.)

3. **What prior knowledge can we assume?**  
   (None; school unit already started; strong related background; multilingual primary-source comfort.)

4. **What should success look like?**  
   (Pass an exam unit; trip or museum prep; general historical literacy; teach others; debate readiness; pure curiosity.)

5. **How much time will they spend with the files?**  
   (≈30 min skim / 1–2 hour session / multi-session course pack.)

6. **What theater or lens should be emphasized?**  
   (Local zoom only after world context; extra weight on military / social / cultural / economic / environmental history.)

7. **What constraints apply?**  
   (Age-appropriate treatment of violence/slavery/religion; avoid graphic detail; length caps; must cover named syllabus points; language for quotes.)

8. **Visual and format preferences?**  
   (Map-heavy vs story-heavy; allow AI reconstructions; prefer museum photos; dyslexia-friendly shorter chapters.)

9. **What would make this lesson a clear “yes” for them?**  
   (Memorable stories, exam dates table, primary voices, “what was happening elsewhere”, revision questions, etc.)

10. **Is there an existing pack to extend?**  
    (Path to prior `./output/<slug>/`, teacher outline, textbook chapter list.)

---

## Strong secondary lanes

Expand each thin primary with 3–6 secondaries. Examples:

### Level & language

- Target reading age or grade band?  
- Define every institution on first use, or assume survey-course vocabulary?  
- Primary sources in translation only, or original + gloss?

### Scope control

- Hard stop year/range?  
- Must-include names (rulers, battles, treaties)?  
- Explicit out-of-scope (e.g. “not the whole Middle Ages — only 1520–1530”)?  
- Comparison cases required (e.g. Rome vs Parthia, not Rome alone)?

### Assessment & use

- Open-book self-study or closed-book exam prep?  
- Need end-of-chapter check questions?  
- Need a one-page “cheat sheet” chapter?

### Sensitivity & honesty

- How to handle contested national narratives?  
- Myths the learner has already heard that need labeling?  
- Trauma-aware compression of mass violence?

### Logistics

- Offline-only (all assets local under `assets/`)?  
- Prefer fewer longer chapters vs more shorter ones?  
- Companion interactive follow-up via `ailc-history` later?

---

## Rabbit holes worth chasing

- **Syllabus mismatch** — user names a battle but needs the diplomatic decade → expand or split chapters  
- **National myth** — learner’s prior story conflicts with mainstream historiography → dedicate a labeled “memory vs record” section  
- **Entity gap** — Wolfram lacks polygons for the year → plan prose + nearby-year map with disclosure  
- **Time budget vs ambition** — two-hour budget cannot host eight deep chapters → cut to 4–5 and move extras to “further paths”  
- **Age gate** — graphic siege/atrocity detail inappropriate → teach stakes without sensational detail  

---

## Status labels for intake answers

| Status | Meaning |
|--------|---------|
| **Resolved** | User answered clearly |
| **Partial** | Working assumption; mark **inferred — confirm** in profile |
| **Unresolved** | Blocking for extensive mode; ask before large write |

Do not invent the learner’s exam date, school curriculum, or trauma boundaries. When inferring level from tone alone, label **Partial**.

---

## Distilled `learner-profile.md` skeleton

```md
# Learner profile — <topic>

## Snapshot
- **Topic:**
- **Level:**
- **Prior knowledge:**
- **Goals:**
- **Time budget:**
- **Constraints:**
- **Visual prefs:**

## Success criteria
- …

## Inferred (confirm)
- …

## Out of scope
- …
```
