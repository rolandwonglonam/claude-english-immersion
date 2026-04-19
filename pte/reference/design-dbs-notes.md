# PTE Skill — Design DBS Notes

> 本文件是 pte skill 设计阶段用 DBS（dontbesilent）拆解思维产出的决策记录。Roland 的指令："DBS 只负责目前阶段设计和结构建设"——意味着 DBS 思维进设计，不进 runtime。这个文件是那次拆解的化石，以后有人（或未来的 Claude）想搞清楚"为什么这个 skill 要这么设计"时，直接读这里。

**日期**：2026-04-15
**触发**：Roland 说"我想在最短时间内学好 PTE，被动学习为主，用 DBS 解决创作问题"
**消解结果**：不改 english-immersion；新建 pte skill 作为 active feeder。

---

## 拆解的模糊词（DBS-deconstruct 思维）

### 1. "学好 PTE" → 原子定义

**表面词**：学好 PTE
**模糊点**：什么叫"学好"？看视频看懂 ≠ 学会；背了模板 ≠ 能用；考到目标分才算，但中间过程如何度量？

**拆解结果**：
- **学好 PTE = Roland 在考场的 4 个 section 都稳定输出符合高分 rubric 的产出**
- **"稳定"的度量 = 日常英语输入里自发使用老师讲的结构规则 ≥ 3 次**（不是刻意练习时用过）
- **"自发"的度量 = Roland 不知道 skill 正在观察时写出来的**（这就是 english-immersion 的 passive runtime 价值）

### 2. "被动学习" → 原子定义

**表面词**：被动学习
**模糊点**：被动到什么程度？skill 完全不打扰？还是临考可以稍微打扰？

**拆解结果**（Roland 后续澄清）：
- 主动 = Roland 必须亲手喂文稿/词汇（这是必然的主动，因为只有 Roland 能做）
- 被动 = English Mode ON 之后，skill 静默读 methodology.md，在日常回复里示范、在 inputs_log 里检测命中
- **"被动"的硬边界：零提醒、零弹窗、零"想练习吗"。只在 Roland 主动 `/pte status` 或 `session-end review` 时才出现进度信息**

### 3. "技巧" → 原子定义

**表面词**：技巧
**模糊点**：技巧 vs 通用英语能力？

**拆解结果**：
- **技巧 ≠ 通用英语能力**（后者 Roland 生活在澳洲已经自然解决）
- **技巧 = PTE 特有的应试结构产出模式**：
  - essay 的段落结构 + thesis 模板
  - SWT 的单句压缩规则
  - DI 的 Overview-Detail-Conclusion 框架
  - signposting 连词、nominalization 密度、ACL collocation 偏好等
- 换句话说，技巧 = 老师视频里讲的那些"外国人日常不这么说、但考试必须这么写"的东西

### 4. "标准" → 原子定义

**表面词**：标准
**模糊点**：标准是 rubric 分数？还是老师讲的 checklist？

**拆解结果**：
- **标准 = 一条可检测的规则 = {id, section, rule, detector, hit_count, status, source, added}**
- **每条规则必须三个属性**：
  1. **可执行**（Roland 看到就知道怎么做）
  2. **可检测**（detector 能靠正则或语义判断是否命中）
  3. **原子化**（一条规则一个动作）
- 这就是为什么 methodology.md 用 YAML list 而不是 markdown prose——每条规则都是结构化对象，能被程序（其实是 Claude 自己）读取和匹配

### 5. "滑动锚点" → 原子定义

**表面词**：滑动锚点
**模糊点**：锚点是什么？影响什么？

**拆解结果**：
- **锚点 = {exam_date, current_level, target_score}** 三变量，存在 state.md
- **锚点影响唯一一件事：english-immersion 回复中"未达标规则"的示范密度**
  - exam_date 越近 → 示范越密
  - target - current 差距越大 → 示范越密
  - exam_date = null → 基线密度（每回复自然 1-2 条）
- **锚点不做其他任何事**（不做提醒、不做 deadline 警告、不做进度条推送）

### 6. "融合" vs "并联" → 归属边界

**Roland 原话**："并列还不够准确，应该是融合"

**拆解结果**：两个 skill 共享同一组数据文件，但职责互补，不重叠：
- **pte** = 主动入口 + 文件写入者（单向写）
- **english-immersion** = 被动 runtime + 文件读取者（读 + 副产品写）
- **共享层 = 数据文件**（state.md / methodology.md / absorption_log.md / pte_vocab.md）

这个结构的好处：pte 和 english-immersion 可以独立演进。改一个不破坏另一个。唯一的耦合点是文件 schema。

---

## 核心架构决策

### 决策 1：不重写 english-immersion

**选项 A**：把所有 PTE 逻辑都塞进 english-immersion（扩展它）
**选项 B**：新建 pte skill 只做 feeder，passive runtime 留给 english-immersion（融合）
**选择**：B
**理由**：
- english-immersion 已经有完整的 ON/OFF 状态机、passive 5 维评分、weekly report 骨架
- 它缺的不是"评分"，而是"外部知识注入口"
- 把"喂料"和"吸收"物理分离，未来换文稿来源（不一定是油管，也可能是 PTE 真题解析）时只改 pte，不动 english-immersion

### 决策 2：`/pte` 空格分子命令 vs `/pte-*` 连字符

**现有**：english-immersion 有 `/pte-essay`、`/pte-summarize`、`/pte-describe`、`/pte-read-aloud`（练习题入口）
**新增**：pte skill 用 `/pte feed`、`/pte anchor`、`/pte rule`、`/pte vocab`、`/pte status`（空格分隔）
**理由**：
- 命名空间不冲突
- 语义上对应："连字符 = 练习题"（单次执行一道题），"空格 = 知识库操作"（feed/query）
- Roland 记忆只需要"`/pte` 开头 + 空格 or 连字符"这一条规则

### 决策 3：DBS 只进设计，不进 runtime

**选项 A**：skill runtime 吃到文稿时自动跑 DBS 拆模糊词（"academic" → 可执行规则）
**选项 B**：DBS 思维由 Claude 在 `/pte feed` 处理文稿时**隐式**应用（不调 /dbs-deconstruct 命令，只是 Claude 自觉把老师的黑话拆到原子级）
**选择**：B
**理由**：
- Roland 原话："DBS 只负责目前阶段设计和结构建设"
- A 会让每次 feed 都走一个完整的 DBS 流程，污染 runtime，且浪费 token
- B 把 DBS 的要求写进 `/pte feed` 的处理原则（"规则必须可执行、可检测、原子化"），Claude 在写规则时自觉遵守

### 决策 4：掌握阈值 = 3 次自发命中

**理由**：
- 1 次 = 偶然
- 2 次 = 巧合
- 3 次 = 模式内化
- 3 次以上 = 过度消耗 Claude 的示范 token（让位给下一条）
- 这个阈值可调，Roland 说改就改

### 决策 5：锚点影响示范密度

**选项 A**：锚点只记录，不影响行为
**选项 B**：锚点影响 english-immersion 回复里未达标规则的示范频率
**选择**：B
**理由**：
- Roland 说"有一个月内拿下考试的目标"，意味着时间紧迫时需要加码
- "加码"的唯一合理形式是让 Claude 自己回复时更密集地示范（因为被动原则禁止主动提醒）
- 没 deadline 时保持基线密度，不盲目加码

---

## 被这个设计刻意放弃的东西

1. **自动评分**：没有数字分数预测。english-immersion 已有 5 维 passive 评分，pte 不重复做。
2. **主动催促**：不推送"该喂新文稿了"。Roland 想喂就喂，不喂就不喂。
3. **文稿全文存档**：pte 不存油管文稿原文。提取完规则后原文就丢。methodology.md 里只留 `source: transcript:{url_or_title}` 作为溯源。
4. **检测器的自动演进**：初期 detector 是粗糙的正则，不做自动学习。日后如果不准，Roland 手动调或在 weekly report 里由 Claude 建议调。
5. **多模态**：不处理视频/音频。必须是文字形态的文稿。

---

## 下次有人回来读这份设计时

如果你发现：
- methodology.md 规则命中率非常低 → detector 可能太严格，先检查 detector 而不是否定整个机制
- Roland 说"为什么 english-immersion 没在示范这些规则" → 先检查 English Mode 是否 ON、methodology.md 是否存在、english-immersion 的 PTE Absorption Hook 段是否还在
- 想加新的数据源（不是油管文稿）→ 只改 `/pte feed` 的输入处理，schema 不动
- 想改阈值 → 改 english-immersion 的 hook 段里的数字常量，不要改 pte（pte 只写数据，不判断阈值）

**不要做的事**：
- 不要让 pte 读 absorption_log.md 或 inputs_log.md（越权）
- 不要让 pte 主动示范规则（越权）
- 不要为了方便把 pte 和 english-immersion 合并（会失去独立演进能力）
