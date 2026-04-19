# PTE Structure Templates — Baseline Rules

> 预置的"通用规则骨架"。处理 `/pte feed` 的文稿时，对照这里已有的规则：如果文稿讲的是同一件事，直接增强现有规则（例如更新检测器或 rule 文本），不要重复创建；如果讲了这里没有的新东西，再新建。

这些是**通用基线**，是 PTE 各子任务在大多数高分策略里都认可的结构。Roland 喂的具体老师的文稿会在这些基线之上做覆盖/细化。

---

## Writing — Essay (Write Essay, 20 min)

### 结构骨架

| 段 | 长度 | 功能 | 规则 id |
| :--- | :--- | :--- | :--- |
| Para 1 Introduction | 2-3 句 | Hook + background + **thesis** | `essay-thesis-template` |
| Para 2 Body 1 | 4-5 句 | Topic sentence + example + elaboration | `essay-body1-signpost` |
| Para 3 Body 2 | 4-5 句 | Counter/second point + example + elaboration | `essay-body2-contrast` |
| Para 4 Conclusion | 2-3 句 | Restate thesis + final stance | `essay-conclusion-opener` |

### 基线规则（作为 methodology.md 初始填充的候选）

```yaml
- id: essay-thesis-template
  section: writing-essay
  rule: "Intro 段末尾必须有明确 thesis，推荐模板: While {counter}, I firmly believe that {main_claim} because {reason1} and {reason2}."
  detector: "paragraph[0] contains ('While ' OR 'Although ') AND ('firmly believe' OR 'strongly argue' OR 'main reason')"
  hit_count: 0
  status: learning
  source: baseline
  added: 2026-04-15

- id: essay-body1-signpost
  section: writing-essay
  rule: "Body 1 首句用 signposting: Firstly / First and foremost / To begin with"
  detector: "paragraph[1] first 2 words ∈ {Firstly,, First and, To begin, The primary, One key}"
  hit_count: 0
  status: learning
  source: baseline
  added: 2026-04-15

- id: essay-body2-contrast
  section: writing-essay
  rule: "Body 2 首句用对比/递进 signposting: On the other hand / Moreover / Furthermore / In contrast"
  detector: "paragraph[2] first 3 words ∈ {On the other, Moreover,, Furthermore,, In contrast,, However,, Nevertheless,}"
  hit_count: 0
  status: learning
  source: baseline
  added: 2026-04-15

- id: essay-conclusion-opener
  section: writing-essay
  rule: "结论段首句用 In conclusion / To sum up / To conclude"
  detector: "last_paragraph first words ∈ {In conclusion, To sum up, To conclude, In summary, Ultimately}"
  hit_count: 0
  status: learning
  source: baseline
  added: 2026-04-15

- id: essay-nominalization
  section: writing-essay
  rule: "每 100 字至少 3 个名词化表达（-tion/-ment/-ance/-sion），避免口语化动词直述"
  detector: "nominalization_count / (word_count / 100) >= 3"
  hit_count: 0
  status: learning
  source: baseline
  added: 2026-04-15
```

---

## Writing — Summarize Written Text (SWT, 10 min)

### 核心规则：**One single sentence, 5-75 words, contains main idea**

```yaml
- id: swt-one-sentence
  section: writing-swt
  rule: "SWT 必须是一个完整句，5-75 词，不能有句号中断（分号可以）"
  detector: "period_count <= 1 AND 5 <= word_count <= 75"
  hit_count: 0
  status: learning
  source: baseline
  added: 2026-04-15

- id: swt-compound-connector
  section: writing-swt
  rule: "SWT 主句 + 从句结构，用 which/while/whereas/although 连接，避免 and/but"
  detector: "contains any of {which, while, whereas, although, despite, despite the fact that}"
  hit_count: 0
  status: learning
  source: baseline
  added: 2026-04-15
```

---

## Speaking — Describe Image (DI, 40 sec)

### 结构：**Overview → 2 key details → Conclusion**（第 5 秒必须开口）

```yaml
- id: di-opening-template
  section: speaking-di
  rule: "开头 5 秒内必须说出图类型: The {chart_type} illustrates / presents / shows {subject}"
  detector: "first sentence contains ('illustrates' OR 'presents' OR 'shows' OR 'depicts') AND one of ('chart', 'graph', 'table', 'diagram', 'map', 'image', 'picture')"
  hit_count: 0
  status: learning
  source: baseline
  added: 2026-04-15

- id: di-two-details
  section: speaking-di
  rule: "必须说出两个具体 detail（数字/比较/趋势），不是泛泛描述"
  detector: "count_of(numeric_or_comparative_phrases) >= 2"
  hit_count: 0
  status: learning
  source: baseline
  added: 2026-04-15

- id: di-conclusion-overall
  section: speaking-di
  rule: "结尾用 Overall / In summary / Thus 给一个总结句"
  detector: "last sentence starts with ('Overall' OR 'In summary' OR 'To summarise' OR 'Thus')"
  hit_count: 0
  status: learning
  source: baseline
  added: 2026-04-15
```

---

## Speaking — Read Aloud (RA, 40 sec)

主要靠发音和流利度，难以通过文本 detector 捕捉。英语 runtime 层面，可以检测 Roland 平时写作是否使用了对应的 prosodic 标记词（如 however/therefore），间接帮他建立"朗读停顿感"。

```yaml
- id: ra-chunking-marker
  section: speaking-ra
  rule: "RA 时在每个逻辑组(chunk)后自然停顿。写作层面: 训练用 semicolon 和逗号标注 chunk 边界"
  detector: "usage_of_semicolons_in_complex_sentences >= 1 per 50 words"
  hit_count: 0
  status: learning
  source: baseline
  added: 2026-04-15
```

---

## Listening — Summarize Spoken Text (SST, 10 min)

```yaml
- id: sst-word-range
  section: listening-sst
  rule: "SST 必须 50-70 词，3-4 句，覆盖 main idea + supporting points"
  detector: "50 <= word_count <= 70 AND 3 <= sentence_count <= 4"
  hit_count: 0
  status: learning
  source: baseline
  added: 2026-04-15
```

---

## 通用跨 section 规则

```yaml
- id: avoid-simple-sentence-streak
  section: all
  rule: "连续 3 个以上简单句 (S-V-O) 视为扣分信号。主动混入 complex/compound sentence"
  detector: "max_consecutive_simple_sentences <= 2"
  hit_count: 0
  status: learning
  source: baseline
  added: 2026-04-15

- id: acl-collocation-preference
  section: all
  rule: "优先使用 ACL 高频搭配 (conduct research not do research, reach a peak not get to the top)"
  detector: "acl_collocation_count / total_verbs >= 0.3"
  hit_count: 0
  status: learning
  source: baseline
  added: 2026-04-15
```

---

## 使用规则

1. **首次 `/pte feed` 前不自动导入这些基线**。Roland 喂第一个文稿时，Claude 先读一遍这份文档，只在确认文稿讲的是同一 section 时用这些基线作为对照框架。
2. **如果 Roland 说"先把基线规则全加进去"**，Claude 把本文档里所有 rules 复制到 `methodology.md` 作为初始填充。
3. **detector 的精度是渐进的**：初期的 detector 可能是粗糙的正则/关键词，随着 hit 数据积累再精细化（这个细化由 english-immersion 在 weekly report 里反馈，不是 pte skill 的工作）。
