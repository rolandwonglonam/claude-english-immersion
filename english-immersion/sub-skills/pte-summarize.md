# /pte-summarize

PTE Academic — Summarize Written Text (SWT) practice task.

## Usage

```
/pte-summarize [passage]
/pte-summarize           # claude picks a passage from the pool
```

## Flow

1. **Present the passage** (~300 words) with this frame:
   - "Read the passage. Summarize it in **one sentence** of 5-75 words. You have 10 minutes. One sentence only — two sentences = automatic 0 on form."
2. **Wait for Roland's summary**. Do not hint at the main idea.
3. **Grade** using `reference/pte-rubrics.md#summarize-written-text`:
   - **First check form**: is it exactly one sentence? is the word count in 5-75? If either fails, Form = 0 and the task score is capped.
   - Content / 2 — does it capture the main idea without off-topic additions?
   - Grammar / 2 — is the single sentence grammatically well-formed?
   - Vocabulary / 2 — word choice, spelling
   - Form / 1
   - **Total / 7**
4. **Report** in this format:

```
PTE SWT Score: {{total}}/7

Form: {{pass/fail}} — {{sentence count}} sentence(s), {{word count}} words
Content: {{n}}/2 — {{what main idea you captured / missed}}
Grammar: {{n}}/2 — {{evidence}}
Vocabulary: {{n}}/2 — {{evidence}}

Biggest single improvement: {{ONE concrete fix}}
```

5. **Log to `pte_progress.md`** and **error patterns to `errors_tagged.md`** as usual.

## Passage pool (seed)

Pick one at random. Each is ~300 words on an academic topic. When you use one, note it used so you don't repeat within a week.

### P1 — Urban heat islands
{{placeholder: short academic passage on urban heat islands. When Roland first runs this command, Claude should generate a fresh ~300-word passage on this topic in PTE register — single paragraph, academic tone, one central argument.}}

### P2 — Microplastic pollution
{{placeholder: similar treatment, marine microplastic research}}

### P3 — The science of habit formation
{{placeholder: neuroscience of habit loops}}

### P4 — Renewable energy grid integration
{{placeholder: technical challenges of wind/solar grid integration}}

### P5 — Archaeology and climate
{{placeholder: how climate records from ice cores rewrite ancient history}}

**Note**: The pool is intentionally small at start. Generate fresh passages as needed — PTE-style means academic register, one clear thesis, compact argumentation, neutral voice.

## Technique coaching (after scoring, not before)

The single-sentence constraint is the hardest part of SWT. After a first attempt, teach Roland these techniques:

1. **Lead with the thesis**, not the topic: "The study found that X causes Y" not "The passage discusses X and Y"
2. **Chain with semicolons and subordinate clauses**: one sentence can legally have 3-4 clauses if joined with `;`, `, which`, `, because`, `although`, `despite`
3. **Cut the author/title reference**: "According to the passage, ..." wastes words — just state the finding

## Anti-patterns

- **Don't write the summary for Roland**. If he asks "what's the main idea?", decline — seeing the idea *is* the exam skill.
- **Don't grade on length alone**. A 30-word summary with the main idea beats a 70-word summary that misses it.
