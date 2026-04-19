# /pte-essay

PTE Academic — Write Essay practice task.

## Usage

```
/pte-essay [topic]
```

If `[topic]` is omitted, Claude picks one from the PTE topic pool (see below).

## Flow

1. **Present the prompt** in PTE format:
   - Topic statement
   - "You have 20 minutes. Write 200-300 words. Argue your position with reasons and examples."
   - Word count reminder
2. **Wait for Roland's essay**. Do not interrupt, do not give mid-writing feedback.
3. **Grade** using `reference/pte-rubrics.md#write-essay`:
   - Content / 3
   - Form / 2 (word count check first — if outside 200-300, entire task caps at 0 on form)
   - Development, structure, coherence / 2
   - Grammar / 2
   - General linguistic range / 2
   - Vocabulary range / 2
   - Spelling / 1
   - **Total / 15**
4. **Report** in this format:

```
PTE Essay Score: {{total}}/15 (≈ PTE Writing band {{approx}})

Content: {{n}}/3 — {{evidence}}
Form: {{n}}/2 — {{word count}} words ({{pass/fail}})
Development: {{n}}/2 — {{evidence}}
Grammar: {{n}}/2 — {{evidence}}
Linguistic range: {{n}}/2 — {{evidence}}
Vocabulary: {{n}}/2 — {{evidence}}
Spelling: {{n}}/1 — {{error count}}

Biggest single improvement: {{ONE dimension + specific fix}}

Errors tagged this task: {{list of error patterns}}
```

5. **Append to `pte_progress.md`**:

```
## YYYY-MM-DD — Essay
Topic: {{topic}}
Score: {{total}}/15
Weakest dimension: {{dimension}}
Patterns: {{list}}
```

6. **Log errors** to `errors_tagged.md` as usual.

## Topic pool (seed, will grow)

These are representative PTE essay topic shapes. Pick one at random if Roland doesn't specify.

1. "Some people believe that university students should be required to attend classes. Others believe that going to classes should be optional. Which view do you agree with?"
2. "In many countries, traditional foods are being replaced by international fast food. Is this a positive or negative development?"
3. "Some argue that governments should fund scientific research regardless of practical application. Others argue that research without commercial value is a waste. Discuss both views and give your opinion."
4. "The rise of remote work has changed how companies hire. To what extent are the benefits of remote work greater than the drawbacks?"
5. "Some believe that the arts (music, painting, literature) should be taught to all children. Others think schools should focus on practical subjects. Discuss."
6. "Environmental problems are too big to be solved by individual action. Do you agree or disagree?"
7. "Social media has fundamentally changed the way young people communicate. Has this been more positive or more negative?"
8. "Some people think that the best way to improve public health is through government regulation. Others think it is through personal education. Discuss both views."

## Anti-patterns

- **Don't give writing tips during the essay**. The real exam gives no hints.
- **Don't round up scores**. If Grammar is a weak 1, score 1 — the honesty is the point.
- **Don't grade content on whether you agree with Roland's position**. Agree or disagree — a clear, supported position scores full content marks regardless of which side.
