# Error Pattern Taxonomy

Growing dictionary of error pattern labels for tagging Roland's English mistakes. Each entry has:

- **Label** (kebab-case, used in `errors_tagged.md`)
- **What it is** (one-line description)
- **Trigger** (the Chinese thinking pattern that produces it)
- **Fix** (the rule, stated as a usage principle not a grammar term)
- **Example**

When a new error shows up that doesn't fit any label here, coin a new one and append it.

---

## Seed patterns (from Roland's 2026-04-12 session)

### preposition-about-vs-with

- **What**: Using "with" where "about" belongs, or vice versa
- **Trigger**: Chinese "跟" maps to both "with" and "about" depending on context
- **Fix**: "with" is the **person** you speak to. "about" is the **topic** you speak on. Different jobs.
- **Example**: "talk with that" → "talk about that". You talk **with me about English**.

### missing-be-verb

- **What**: Dropping "am/is/are" before a verb ending in -ing
- **Trigger**: Chinese doesn't have the "to be" auxiliary — "我学英语" doesn't need "am"
- **Fix**: Present continuous always needs an engine: **am / is / are + -ing**. No engine = no sentence.
- **Example**: "cause I learning English" → "cause I'm learning English"

### embedded-question-word-order

- **What**: Keeping question word order inside a subordinate clause
- **Trigger**: Chinese uses the same word order for questions and statements, so the flip feels unnatural
- **Fix**: When a question lives inside another sentence, drop it back to statement order. Questions: "What does that mean?" Embedded: "I need to know what **that means**."
- **Example**: "I need to know what's that mean" → "I need to know what that means"

---

## Common Chinese-English patterns (seeded, will grow)

### article-missing-a

- **What**: Dropping "a/an" before a singular countable noun
- **Trigger**: Chinese has no articles
- **Fix**: Countable singular nouns almost always need **a / an / the**. Default to "a/an" when introducing something new.
- **Example**: "I have cat" → "I have a cat"

### article-missing-the

- **What**: Dropping "the" before something already known
- **Trigger**: Chinese has no articles
- **Fix**: Use "the" when both speaker and listener already know which one. "Close the door" (the one we both see).
- **Example**: "I fixed bug in login page" → "I fixed the bug on the login page"

### plural-missing-s

- **What**: Forgetting to pluralize countable nouns
- **Trigger**: Chinese plurals are optional and usually implicit
- **Fix**: If there's more than one, the noun gets "-s". No exceptions in casual register.
- **Example**: "three customer" → "three customers"

### verb-tense-flat

- **What**: Using present tense where English demands past
- **Trigger**: Chinese relies on time words ("昨天") instead of verb inflection
- **Fix**: If it happened and finished before now, use past tense. The time word is not enough.
- **Example**: "yesterday I go to the meeting" → "yesterday I went to the meeting"

### to-plus-ing

- **What**: Writing "to + verb-ing" when the rule is "to + bare verb"
- **Trigger**: "help me to refactoring" sounds like "help me in refactoring"
- **Fix**: After "to" in an infinitive, use the bare verb. "to refactor", "to learn", "to go". Not "to refactoring".
- **Example**: "help me to refactoring this" → "help me refactor this" (or "help me with refactoring this")

### third-person-s

- **What**: Forgetting the "-s" on third-person singular present verbs
- **Trigger**: Chinese verbs never inflect for person
- **Fix**: He/she/it + verb → verb gets an -s. "He runs." "It works."
- **Example**: "he run fast" → "he runs fast"

### pronoun-it-missing

- **What**: Dropping "it" where English requires a placeholder subject
- **Trigger**: Chinese drops pronouns freely
- **Fix**: English sentences need a subject, even an empty one. "It's raining." "It seems fine."
- **Example**: "is raining" → "it's raining"

### word-order-adjective-after-noun

- **What**: Putting the adjective after the noun (Chinese default)
- **Trigger**: "一个红的苹果" — descriptor comes first in Chinese too, but modifier particles confuse the mapping
- **Fix**: English adjective goes **before** the noun. "red apple", not "apple red".
- **Example**: "a function recursive" → "a recursive function"

### collocation-wrong-partner

- **What**: Pairing a correct word with the wrong collocational partner — technically grammatical but sounds unnatural
- **Trigger**: Translating Chinese verb+noun combinations word-by-word ("做研究" → "do research" when native English uses "conduct research")
- **Fix**: Look up the headword in `pte_vocab.md` (the Academic Collocation List). The ACL tells you which specific partner English natives reach for. Collocations are fixed — "heavy rain" not "strong rain", "make a decision" not "do a decision", "reach a peak" not "arrive at a peak".
- **PTE impact**: High — collocations are directly rewarded in Writing (vocabulary range, linguistic range) and in Reading/Listening Fill-in-the-Blanks tasks
- **Example**: "do research" → "conduct research"; "big growth" → "significant growth"; "make a crime" → "commit a crime"

---

## How to add a new pattern

When an error doesn't match any label above:

1. Coin a label in kebab-case (short, memorable)
2. Append a new section to this file with the 5 fields (what/trigger/fix/example)
3. Reference the label in `errors_tagged.md` for that error entry

The taxonomy growing is a feature — it maps the territory of Roland's specific mistakes.
