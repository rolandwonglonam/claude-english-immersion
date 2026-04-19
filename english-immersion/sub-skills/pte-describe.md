# /pte-describe

PTE Academic — Describe Image practice task.

## Usage

```
/pte-describe [image-path]
/pte-describe              # claude generates a text-based "describable" scenario
```

Roland can pass a local image path (Claude reads it via the Read tool) or ask for a text-scenario when no image is handy.

## Flow

1. **Present the image** or the text-scenario:
   - "You have 25 seconds to study it. Then speak for 40 seconds — stop at the timer. Cover: what it is, the 2-3 most notable features, and an overall conclusion."
   - Remind Roland of the 4-sentence template (see below).
2. **Wait for Roland's response**. He can either:
   - Type what he said
   - Record himself, transcribe, paste transcript
   - Paste his speaking notes
3. **Grade** using `reference/pte-rubrics.md#describe-image`:
   - **Content / 5** — Claude can score this from the transcript
   - **Oral fluency / 5** — Claude asks Roland to self-rate and records the answer
   - **Pronunciation / 5** — Claude asks Roland to self-rate and records the answer
4. **Report** in this format:

```
PTE Describe Image Score: {{total}}/15

Content: {{n}}/5 — {{what you covered / missed}}
  ✓ stated what it is: {{yes/no}}
  ✓ called out highest/lowest: {{yes/no}}
  ✓ mentioned trend or pattern: {{yes/no}}
  ✓ stated a conclusion: {{yes/no}}
  ✓ within 40 seconds: {{yes/no, self-reported}}

Oral fluency (self-rated): {{n}}/5
Pronunciation (self-rated): {{n}}/5

Biggest single improvement: {{ONE concrete fix}}
```

5. **Log to `pte_progress.md`**.

## The 4-sentence template (drill this)

PTE Describe Image in 40 seconds does not allow free-form description. You need pre-built scaffolding:

1. **What it is**: "This is a {{chart/graph/map/image}} showing {{X}} over/across {{Y}}."
2. **Most notable feature**: "The highest value / most notable point is {{A}} at {{B}}."
3. **Contrast or trend**: "In contrast, {{C}} is {{D}}." OR "Overall the values trend {{upward/downward/stable}}."
4. **Conclusion**: "This suggests {{the insight the chart supports}}."

Four sentences. Pre-chunked. Nothing fancy. The exam rewards hitting the structure.

## When there's no image

Claude can generate a text-scenario like:

> "Imagine a bar chart titled 'Monthly solar panel installations in Australia, 2023'. The x-axis is months Jan-Dec. The y-axis is number of installations, from 0 to 12,000. January sits around 4,500. July peaks at 11,800. December is around 6,200. Overall trend: rising from Jan to Jul, then declining through Dec."

Then treat Roland's response as if he were describing an image.

## Image reading (when given an image path)

Claude reads the image via the Read tool, describes it to itself mentally, then scores Roland's description against what's in the image. Do NOT tell Roland what's in the image before he attempts — that defeats the practice.

## Anti-patterns

- **Don't describe the image for Roland**. His job is to generate the description.
- **Don't accept a description without a stated conclusion**. "Conclusion" is the highest-value sentence — the exam looks for it.
- **Don't grade oral fluency from the transcript**. The transcript has no timing info. Only self-rating captures pause length and pace.
