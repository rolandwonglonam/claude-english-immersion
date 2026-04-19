---
name: pte
description: Active feeder for the user's PTE knowledge base. Ingests YouTube transcripts from PTE teachers, extracts structured rules, and writes them to methodology.md — which english-immersion reads passively during English Mode ON to silently teach the user PTE techniques through daily conversation. Activate on `/pte`, `/pte feed`, `/pte anchor`, `/pte rule`, `/pte vocab`, `/pte status`, or when the user says "喂 pte 文稿"/"加一条 PTE 规则"/"设置 PTE 锚点"/"看 PTE 吸收进度". Do NOT activate during content production in your native language. This skill is fire-and-forget: it only writes data files, it does not run any passive loop — all passive behavior belongs to english-immersion.
---

# PTE Skill — Active Feeder

the user 的 PTE 知识弹药库。这个 skill 只做"主动喂"：收文稿、提规则、手补、写锚点。**被动吸收完全交给 english-immersion**——那个 skill 的 ON-mode runtime 会自动读本 skill 写的文件，在日常对话中静默示范 + 检测命中。

## 边界声明（非常重要）

- **pte 不常驻**：跑完一次 `/pte <子命令>` 就退出。没有 session state、没有 passive loop、没有提醒。
- **pte 不纠错**：the user 日常英语输入的 passive 检测由 english-immersion 负责。
- **pte 不打扰**：永远不主动推送 "想喂点内容吗" "PTE 进度看一下吧"。
- **english-immersion OFF 时 pte 依然可用**：the user 可以在白天任意时刻丢一个文稿过来。data 会写好，等 English Mode 下次 ON 时自动激活。
- **设计阶段交付 DBS**：本 skill 的核心概念拆解已固化在 `reference/design-dbs-notes.md`，以后回看不用重拆。

## 数据契约（与 english-immersion 共享的文件）

| 文件 | 归属（谁写） | 谁读 |
| :--- | :--- | :--- |
| `data/state.md` 的 `sliding_anchor` 段 | pte 写 | english-immersion 读（判定"未达标规则的示范密度"） |
| `data/methodology.md` | pte 写（ingest/手补） | english-immersion 读（rule match + 回复示范） |
| `data/pte_vocab.md` | pte 追加 + user 手补 | english-immersion 读（ACL 偏好） |
| `data/absorption_log.md` | english-immersion 写（命中时） | Weekly report 读 |

pte 永远不读 absorption_log.md，不读 inputs_log.md，不读 errors_tagged.md——那些是 english-immersion 的势力范围。

---

## 子命令

### `/pte anchor` — 滑动锚点

首次运行时必问三个问题，后续可随时重设：

1. **考试日期**（可以说"还没定"→ 存 `null`）
2. **当前水平**（上次考的分数或自评 PTE 分数）
3. **目标分**

写入 state.md 的 `sliding_anchor` 段（见下文 state.md schema）。

**锚点的作用**（只是给 english-immersion 读的指示器）：
- `exam_date` 越近 → english-immersion 回复中，未达阈值规则的示范密度越高
- `target - current` 越大 → 同上，强度加码
- 无日期 → 线性基线密度（每回复 1-2 条未达标规则的自然示范）

### `/pte feed <文稿>` — 喂油管文稿

the user 粘贴 PTE 老师讲解视频的文稿（Writing/Speaking/Reading/Listening 任一 section 的技巧讲解）。

**Claude 处理流程**：

1. **识别 section**：判断文稿在讲哪个 PTE 子任务（Essay / SWT / DI / RA / Repeat Sentence / ...）
2. **提取结构化规则**：从文稿里提炼可执行规则。每条规则必须满足：
   - **可检测**（写在 inputs_log 里的句子，靠正则或语义匹配能判断是否命中）
   - **可执行**（the user 看到就知道怎么做，不是"要 sophisticated"这种黑话）
   - **原子级**（一条规则一个动作，不堆叠）
3. **展示给 the user 确认**，格式：

   ```
   从文稿提取 N 条规则（section: {{Writing Essay}}）：

   1. [thesis-while-y] Essay 第一段必须含 thesis，模板: While {{counter}}, {{main}} is more important because {{reason}}.
      → 检测: 第一段出现 "While " + "more important" 或 "primarily because"
   2. [signpost-firstly] 主体段开头用 signposting 连词 (Firstly/Moreover/Furthermore)，不用 And/Also。
      → 检测: 段落首词 ∈ {Firstly, Moreover, Furthermore, Additionally, In contrast}
   3. ...

   Approve all / 删 [编号] / 改 [编号] / 加 [新规则] / 全弃
   ```

4. **the user 确认后**，追加到 `methodology.md`（见 schema）。新规则的 `hit_count` 初始 = 0、`status` = `learning`。
5. **不要自动调用 english-immersion**。pte 写完文件就结束，不触发任何 review / 评估 / 示范。

**"结构化规则"示范库**：见 `reference/structure-templates.md`，预置了 Essay / SWT / DI / RA 四个子任务的通用规则骨架，处理文稿时可对照增量补充。

### `/pte rule <规则文本>` — 手动追加一条规则

the user 直接口述一条规则，skill 帮他格式化（补 section / ID / 检测器），追加到 methodology.md。不走 feed 的"提取-展示"流程，直接写。

用法示例：
> `/pte rule Writing Essay 结论段第一句必须用 In conclusion 或 To sum up`

Claude 生成：
```yaml
- id: conclusion-opener
  section: writing-essay
  rule: "结论段首句用 In conclusion / To sum up"
  detector: "结论段首词组 ∈ {In conclusion, To sum up, To conclude}"
  hit_count: 0
  status: learning
  source: manual
  added: 2026-04-15
```
追问一句"有要补的吗？"再落盘。

### `/pte vocab <词列表>` — 追加高频词/collocation

追加到现有 `pte_vocab.md`（不是覆盖），每行一个 collocation。english-immersion 已经在读这个文件。

### `/pte status` — 查看进度（the user 主动查，不推送）

输出一屏内摘要：

```
Anchor: exam {{date}} | current {{score}} | target {{target}} | {{days_left}} days
Methodology: {{total}} rules | {{mastered}} mastered ({{pct}}%) | {{learning}} learning
Top 5 未吸收规则（按 hit_count 升序）:
  - [id] rule text — 0 hits
  - ...
Recent hits (last 7 days): {{count}}
```

---

## 文件 schema

### `state.md` 的 sliding_anchor 段（pte 写）

追加到现有 state.md 底部：

```markdown
## Sliding Anchor (pte skill)

- exam_date: 2026-05-15   # or null if undecided
- current_level: 60       # PTE overall score, 0-90
- target_score: 70
- set_at: 2026-04-15
- notes: (optional free text)
```

english-immersion 读这几个字段判断示范密度。

### `methodology.md` schema

YAML list，每条规则一个 entry：

```yaml
---
# PTE Methodology — rules distilled from transcripts + manual additions
# Read by english-immersion during ON mode for passive rule-match and demonstration.
# Mastered rules (hit_count >= 3) are retired from active demonstration to make room for new ones.
---

rules:
  - id: thesis-while-y
    section: writing-essay
    rule: "Essay 第一段必须含 thesis，模板: While {counter}, {main} is more important because {reason}."
    detector: "first_paragraph contains 'While ' AND ('more important' OR 'primarily because')"
    hit_count: 0
    status: learning   # learning | mastered | retired
    source: transcript:{video_title_or_url}   # or "manual"
    added: 2026-04-15
    mastered_at: null
```

**状态转换**：
- `learning` → `mastered`：当 `hit_count >= 3`
- `mastered` → `retired`：当 rule 达成 7 天后无新命中（english-immersion weekly 检查时执行）

### `absorption_log.md` schema（english-immersion 写，pte 只读说明）

```markdown
# Absorption Log — append-only hit stream

## 2026-04-16 09:30
- rule: thesis-while-y
- input: "While cost is a concern, quality is more important because..."
- snippet: "While cost is a concern..."
- new_hit_count: 1
```

---

## 与 english-immersion 的 Hook（对方 SKILL.md 里追加的最小指令）

本 skill 不修改 english-immersion 的任何原有逻辑，只在对方 SKILL.md 末尾追加一段 "PTE Absorption Hook"，大意：

> 如果 `methodology.md` 存在且 English Mode 为 ON：
> 1. 写 inputs_log 时，对每条 `status: learning` 的规则跑 detector，命中就 append 到 absorption_log.md 并 hit_count += 1
> 2. 达 3 次 → status 改 mastered、mastered_at 写当前日期
> 3. 自己生成英文回复时，偏向使用 hit_count 最低的 2-3 条 learning 规则（示范给 the user 看），sliding_anchor.exam_date 越近示范越密
> 4. Weekly report 多一段 "Absorption"：mastered / learning / 本周命中次数 / top 3 未吸收

具体文本由 english-immersion 的 SKILL.md 持有，pte 不负责维护。

---

## 非目标（pte 刻意不做的事）

- **不做日常英语纠错**（english-immersion 的事）
- **不做 session-end review**（english-immersion 的事）
- **不写 inputs_log/errors_tagged/words_learned**（english-immersion 的事）
- **不主动提醒喂文稿**（被动原则）
- **不跑 `/pte-*` 练习题**（那是 english-immersion 的 sub-skills：`/pte-essay`、`/pte-summarize` 等，与本 skill 不冲突，两者可共存）
- **不做 OCR/ASR**：文稿必须 the user 已经是文字形态粘贴进来

## 与 english-immersion 的 `/pte-*` 子命令区别

| 命令 | 所属 skill | 类型 | 作用 |
| :--- | :--- | :--- | :--- |
| `/pte feed` / `/pte rule` / `/pte anchor` / `/pte vocab` / `/pte status` | **pte**（本 skill） | 主动喂料 | 往知识库写东西 |
| `/pte-essay` / `/pte-summarize` / `/pte-describe` / `/pte-read-aloud` | english-immersion | 主动练习 | 跑一次模拟题并打分 |

两套命令都是"主动入口"，都是 pull-only。区别在：前者往知识库加东西，后者用知识库考自己。命名上用 `/pte xxx`（空格）vs `/pte-xxx`（连字符）明确区分。
