# Claude English Immersion + PTE Skills

Turn your daily Claude Code sessions into passive English immersion and PTE (Pearson Test of English) exam prep.

## What This Is

Two Claude Code skills that work together:

- **english-immersion** — The runtime. When English Mode is ON, Claude replies in English, silently logs your errors, teaches unknown words Wittgenstein-style (usage, not translation), and delivers batch error reviews at session end. No per-sentence nagging.
- **pte** — The feeder. Ingest PTE teacher transcripts (YouTube, etc.), extract structured rules, and write them to a methodology file. english-immersion reads these rules passively and demonstrates them in its replies — you absorb PTE techniques without active study.

## Core Idea

If you already talk to Claude every day, switching that conversation to English gives you hours of passive input per day without adding anything to your schedule. This skill makes that switch frictionless.

## Features

- Binary ON/OFF state machine (say "english mode on" / "english mode off")
- Input logging (append-only, English/mixed inputs only)
- Error tagging with pattern taxonomy (e.g., `missing-be-verb`, `preposition-about-vs-with`)
- Wittgenstein-style word teaching: usage + example + contrast, no translation
- Session-end batch error review (not inline correction)
- Weekly reports with error frequency, new words, PTE progress
- PTE Absorption Hook: passive rule demonstration in Claude's replies
- Active PTE practice sub-commands: `/pte-essay`, `/pte-summarize`, `/pte-describe`, `/pte-read-aloud`
- Sliding anchor: exam date proximity drives demonstration density

## Installation

### 1. Copy skills to Claude Code

```bash
# Copy both skill directories to your Claude Code skills folder
cp -r english-immersion ~/.claude/skills/
cp -r pte ~/.claude/skills/
```

### 2. Set up data directory

Copy the data template to wherever you keep persistent files (e.g., your Obsidian vault, a notes folder, etc.):

```bash
cp -r data-template ~/path/to/your/data/english-immersion/
```

### 3. Update paths in SKILL.md

Edit both `SKILL.md` files and replace `data/` with the actual path to your data directory:

```
# In english-immersion/SKILL.md and pte/SKILL.md
# Replace all occurrences of:
data/
# With your actual path, e.g.:
~/notes/english-immersion/
```

### 4. Customize context

Edit `english-immersion/SKILL.md` line 10 — replace the placeholder with your actual context:

```
**Context**: [Your location]. Target: PTE [score]+ in all four sections. Baseline: [your baseline]. Timeline: [your timeline].
```

## Usage

### English Mode

```
"english mode on"    → Claude switches to English replies + starts logging
"english mode off"   → Back to normal
```

### PTE Feeding

```
/pte feed <paste a PTE teacher transcript>   → Extract rules to methodology.md
/pte rule <one rule in plain text>           → Manually add a rule
/pte anchor                                  → Set exam date + current/target score
/pte vocab <collocation list>                → Add high-frequency collocations
/pte status                                  → View progress summary
```

### PTE Practice (optional, pull-only)

```
/pte-essay [topic]       → 200-300 word argumentative essay
/pte-summarize [text]    → Single-sentence summary (SWT task)
/pte-describe [image]    → 40-second image description (DI task)
/pte-read-aloud [text]   → Calibrated read-aloud passage (RA task)
```

## Architecture

```
english-immersion/          ← Runtime skill (always-on when mode: on)
├── SKILL.md                   Main spec + PTE Absorption Hook
├── reference/
│   ├── wittgenstein-style.md  Word teaching methodology
│   ├── error-pattern-taxonomy.md  Error classification system
│   └── pte-rubrics.md        PTE scoring guides
└── sub-skills/
    ├── pte-essay.md           Write Essay practice
    ├── pte-summarize.md       Summarize Written Text practice
    ├── pte-describe.md        Describe Image practice
    └── pte-read-aloud.md      Read Aloud practice

pte/                        ← Feeder skill (fire-and-forget)
├── SKILL.md                   Ingest transcripts, extract rules
└── reference/
    ├── structure-templates.md Rule templates per PTE task
    ├── exam-checklist.md      Exam day checklist
    └── design-dbs-notes.md    Design decisions archive

data/                       ← Your persistent data (lives in your vault/notes)
├── state.md                   ON/OFF + sliding anchor
├── inputs_log.md              Raw English inputs (append-only)
├── errors_tagged.md           Tagged errors with patterns
├── words_learned.md           Wittgenstein-style word cards
├── methodology.md             PTE rules (written by /pte, read by english-immersion)
├── absorption_log.md          Rule hit stream (written by english-immersion)
├── pte_vocab.md               Academic Collocation List
├── pte_progress.md            Practice scores over time
└── weekly_reports/            Weekly summaries
```

## How the Two Skills Work Together

1. **pte** writes rules to `methodology.md` (from YouTube transcripts or manual input)
2. **english-immersion** reads `methodology.md` during ON mode
3. When you type English, english-immersion checks if your input matches any learning rules → logs hits to `absorption_log.md`
4. When Claude replies, it silently demonstrates the least-hit rules in natural phrasing
5. After 3 spontaneous hits, a rule graduates to "mastered"
6. Weekly reports track absorption progress

## License

MIT
