# /pte-read-aloud

PTE Academic — Read Aloud practice task.

## Usage

```
/pte-read-aloud             # claude picks a difficulty-calibrated passage
/pte-read-aloud [difficulty] # difficulty: easy / medium / hard
```

## Flow

1. **Generate a 60-word passage** calibrated to the current week (see progression in `reference/pte-rubrics.md#read-aloud`):
   - **Week 1-2 (easy)**: concrete topics, common vocabulary, short sentences
   - **Week 3-4 (medium)**: abstract topics, mixed vocabulary, compound sentences
   - **Week 5+ (hard)**: academic register, Latinate vocabulary, complex sentence structures
2. **Present the passage** with this frame:
   - "You have 30-40 seconds to prepare, then 35-40 seconds to read aloud. Ideally record yourself — we'll score from your self-report."
3. **Flag high-risk words** before Roland starts: long syllable counts, unusual stress patterns, Latinate roots. Example: "Watch these: `pharmaceutical` (4 syllables, stress on -ceu-), `infrastructure` (stress on in-), `predominantly` (stress on -dom-)."
4. **Wait for Roland's self-report**:
   - Any word he tripped on?
   - Any sentence he restarted?
   - Self-rated fluency 1-5?
   - Self-rated pronunciation 1-5?
5. **Grade**:

```
PTE Read Aloud Score: {{total}}/15

Content (self-reported accuracy): {{n}}/5
  Words tripped on: {{list}}
  Sentences restarted: {{count}}

Oral fluency (self-rated): {{n}}/5
Pronunciation (self-rated): {{n}}/5

Flagged words — drill individually:
- {{word 1}}: {{phonetic breakdown + stress}}
- {{word 2}}: {{phonetic breakdown + stress}}
```

6. **Log to `pte_progress.md`**.

## Difficulty examples

### Easy (W1-2)

> Australia has more than twenty national parks that protect unique wildlife. Many visitors come each year to see kangaroos, koalas, and native birds. Park rangers work hard to keep these animals safe from threats like bushfires and habitat loss. Visitors are asked to stay on marked trails and not feed the animals, which helps protect the natural balance of the parks.

Common words, short sentences, no Latinate clusters.

### Medium (W3-4)

> The rise of remote work has prompted companies to reconsider traditional office layouts. Many firms now offer flexible arrangements, allowing employees to work from home several days each week. This shift has reduced commuting costs and improved work-life balance for many, although it has also raised concerns about collaboration and the development of younger employees who benefit most from in-person mentorship.

Abstract topic, compound sentences, some multisyllabic vocabulary.

### Hard (W5+)

> Recent archaeological evidence suggests that the decline of certain Mesoamerican civilizations was precipitated by prolonged droughts rather than by external invasion. Paleoclimatic reconstructions, derived from stalagmite isotope ratios in regional caves, indicate sustained rainfall anomalies spanning several decades. These environmental pressures likely exacerbated existing sociopolitical tensions, accelerating the abandonment of major urban centres throughout the classical period.

Academic register, Latinate density, complex noun phrases — close to real PTE difficulty.

## Anti-patterns

- **Don't read the passage for Roland**. If he asks "how do you pronounce X?", give the stress pattern in words ("like AD-van-tage") — do not audio-read.
- **Don't over-inflate self-ratings**. If Roland says "I think 4/5 on fluency but I paused twice", that's a 3. Be the honest coach.
- **Don't skip the high-risk word flag**. Flagging them *before* reading gives Roland a chance to mentally rehearse — that's real practice.
