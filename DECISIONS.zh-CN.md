# 设计决策

借了什么、改了什么、新发明了什么、否决了哪些备选项。要 fork 这个 repo，先读这篇 — 它告诉你哪些部件是承重的，哪些可以替换。

> 🇬🇧 English version: [DECISIONS.md](DECISIONS.md)

---

## 来源

这套系统是两个先行想法的融合，加上为了让它们作为一个工作品配合起来必须新发明的连接组织。

### 来自 Andrej Karpathy — 「LLM Wiki」概念
- 类型化页面（vs. 自由形式笔记）
- Agent-readable schema（`AGENTS.md`）
- 质量优先于数量（拒收低价值材料）
- Wiki 作为 *agent 运行的底层*，不只是个搜索目标

### 来自 Jeff Su — 「Personal OS / Cowork OS」模式
- Workstation（带自己 CLAUDE.md 的子系统）
- Hot cache 保证 session 间连续性
- Plain-language 接口（用户用人话，不用 config）
- 五层 pipeline（Input → Knowledge → Skills → Action → Output）

### 为了让两者配合，我新发明（或借来重塑）的部分
- **4 题 Quality Gate** — 离散、可打分、阻止进入 Permanent
- **三 zone ingest 格式**（verbatim / metadata / decomposed SOP）
- **Zone C 拆解** — 把方法论转成 SOP 的 6 步变换
- **Spotlight Rule** — 配带校准日志的防谄媚机制
- **Disposable Noise 拒收清单** — 显式列出不归档的类别
- **带沙箱约束的多 vault routing**（constellation 模式，单独放进 `docs/extending/`）
- **质量门前置的 ingest**，而不是「先捕获后整理」

---

## 决策清单（大致按时间）

### D-001: 纯 Markdown，无数据库
**状态：** 接受。
**背景：** 大多数「第二大脑」系统会把你锁死在某个工具里（Notion、Roam、Tana、带重插件的 Obsidian）。我要的是可移植性：任何 agent、任何编辑器、任何同步方式。
**决策：** 只有文件。文件夹约定 + frontmatter 就是 schema。
**取舍：** 失去：属性查询、双向图视图、插件生态。得到：可移植、版本控制友好、agent 可读。
**否决的备选：**
- Notion API + LLM（和 Notion 演化中的 API 表面耦合太深）
- SQLite 后端的 wiki（个人级别 over-engineering）
- 重插件栈的 Obsidian vault（Obsidian-specific lock-in）

### D-002: 五层，不是三层也不是七层
**状态：** 接受。
**背景：** Karpathy 的模型是 3 层（Input → Knowledge → Output）。Jeff Su 的是 5 层（Skills 和 Action 是分开的）。
**决策：** 五层。Skills（`Software/`）和 Action（`LifeOS/`）和 Knowledge 的留存规则、读者、更新节奏都不一样 — 合在一起会稀释类型信号。
**取舍：** 更多文件夹要记。但「这东西放哪？」这个问题反而更好回答了，因为边界更锐利。

### D-003: Quality Gate 在 *入库之前* 跑，不是在入库后
**状态：** 接受。
**背景：** 默认捕获优先的系统（Roam、Logseq、Notion）优化了低摩擦捕获。代价是一堆平庸页面的坟场。
**决策：** 没过门的进 `_Queue/` 等 30 天。它们直到 `/promote` 在 fresh session 里验证过才能进 Permanent。
**为什么重要：** 这是整个系统里**杠杆最高**的一条规则。大多数「第二大脑」的痛苦来自低质量内容淹没高质量内容。
**否决的备选：**
- 先全收再筛（指望你以后会有的动力，不会有）
- 检索时再筛（太晚了 — 那时噪音已经吃掉了搜索时间）

### D-004: Promote 必须在 *fresh* session
**状态：** 接受。
**背景：** 刚 ingest 完某项的 LLM 对这项有过高估值的偏见。
**决策：** Promotion 要求开一个新 session，agent 除了队列项和评分标准外没有任何上下文。
**取舍：** 增加摩擦。但摩擦就是重点 — 它是对你自己热情的健全性检查。

### D-005: Zone C 拆解（6 步 SOP 变换）
**状态：** 接受。
**背景：** 大多数 knowledge base 页面会漂到「这篇文章讨论了 X」的 summary 模式，到你真要 *用* 的时候没用。
**决策：** 方法论类源材料必须做 6 步拆解：Core Question → Cut the Fluff → Plain-Language Translation → Reverse-Engineered SOP → Failure Modes → Critical Review。
**拒绝启发式：** 如果你发现自己在写「这篇文章讨论了 X」，停下，问「读者周一早上要 *做* 什么因为这个？」
**取舍：** 拆解比 summary 多用 5–10 倍的 token。对于你真会用的页面，值。

### D-006: 三因子加权评分的 Spotlight Rule
**状态：** 接受。
**背景：** LLM 的默认行为就是浮出一堆「你可能觉得有意思」 — 讨好、没用、训练你忽略 assistant。
**决策：** 只有当（Leverage × 0.4）+（Specific Knowledge × 0.3）+（Surprise × 0.3）≥ 7.0 **且** 置信度 ≥ medium 时才 spotlight。每 session 最多 3 个。
**防谄媚机制：** Surprise 维度强迫 agent 给「打破用户既有模型」的洞见打分。只确认用户已知信念的 spotlight 凑不到 7.0。
**校准 loop：** 每个 spotlight 进日志，带空白的 `outcome:` 字段。30 条后扫一遍 — <30% 命中率 → 阈值上调；>70% → 下调。
**取舍：** Assistant 变安静。有些用户会误判成「Claude 变差了」。这是正确的取舍。

### D-007: 默认路由是 Queue，不是 Permanent
**状态：** 接受。
**背景：** 见 D-003。偏好应该偏向 Permanent 里 *更少*，不是更多。
**决策：** 即便过门的 item，如果 agent 对层归属不确定，也先进 queue。Promotion 永远是显式的。

### D-008: Lint 不自动修
**状态：** 接受。
**背景：** 自动修很诱人（agent 可以重命名页面、补 broken link、去重）。这是陷阱 — agent 对哪个页面是 canonical 的心智模型经常是错的。
**决策：** Lint 只报告。人修。或者人看完报告后说「按推荐方案走」。

### D-009: 默认单 vault，constellation 是 opt-in
**状态：** 接受。
**背景：** 真实的 founder workflow 通常会长成 Personal + Company + Private 三个 vault，带跨 vault routing、沙箱约束、敏感度自动路由。
**决策：** Template 出厂只有单 vault。多 vault 的模式记在 `docs/extending/multi-vault.md`，给长大了的用户。
**取舍：** Demo 略不炸眼。但 onboarding 大幅简化，80% 用户也用不到多 vault。

### D-010: Workstation 是带自己 CLAUDE.md 的文件夹
**状态：** 接受。
**背景：** 有些任务（起草邮件、写长文、做教程）需要 *叠加* 规则：根宪法 + 域特定的 voice 和 workflow。
**决策：** Workstation 是文件夹。它的 CLAUDE.md 叠在根之上。根管通用规则；workstation 管域规则。
**模式：** 每个 workstation 有 `CLAUDE.md`、`MEMORY.md`、`<Name> Resources/`。
**取舍：** 文件更多。但可扩展 — 加 workstation 不需要改根宪法。

### D-011: 全 17 个 skill 都发，不只发 minimal 核心
**状态：** 接受。
**背景：** 拿到 17 个 skill 的用户会用 5 个。只拿到 5 个的用户永远发现不了另外 12 个。
**决策：** 全发；在 README 里说清楚哪 5–7 个是核心、哪些是情境性的。用户自己删不要的。

### D-012: Spotlight Rule 是核心，不是可选
**状态：** 接受（有争议）。
**背景：** 它是系统里最 opinionated 的一块。有些用户会讨厌。
**决策：** 核心默认。README 明示这是 opinionated，用户可以注释掉 `CLAUDE.md` 里那一段来停用。
**理由：** 系统的主要差异化点就是 *治理* — 防止 wiki 腐烂的 opinionated 规则。把最 opinionated 的那块去掉 = 阉割了差异化。

### D-013: MIT 协议
**状态：** 接受。
**背景：** 在 MIT、Apache 2.0、CC BY-SA、CC0 之间选。
**决策：** MIT。
**理由：** 最大化复用，含商用。Apache 2.0 的专利授予条款这里不相关（没什么发明）。CC BY-SA 对衍生作品的协议限制会伤害采用率。CC0 放弃署名权。

### D-014: 积极维护，PR welcome
**状态：** 接受。
**背景：** 维护姿态在 active vs. reference-only vs. snapshot 之间选。
**决策：** Active，配显式的 PR template 和贡献者指南。
**取舍：** 持续维护成本更高。但用户用边缘 case 推回来时系统会变更好。

---

## 还没想好的几件事

- **`voice-extract` 是核心还是 optional-modules？** 它是最个人化的一层。目前是核心，因为 README 明示第一天就要跑。
- **`/connections` 是周度还是按需？** 目前按需。有些用户会受益于定时跑 `/connections`；我还没决定要不要发一个 cron 风格的调度 helper。
- **多语言 voice principles。** 系统假设每个用户一套 voice，但双语创作者可能要两套。还没解决。

如果你对这几件有想法，开 issue。

---

## 被否决的、值得留名的想法

- **图视图。** 诱人，但图是隐式的（在 `[[wikilinks]]` 和 frontmatter 里）。做可视化是另一个工具，不是 wiki 的功能。
- **AI 自动打标签。** 试过；agent 会飘到过度打标（每个页面都得「productivity」「ai」「knowledge」）。手动 + lint 找漏更靠谱。
- **强制双向链接。** 强迫到处对称 wiki 风格 link。在个人尺度上代价大于收益。
- **REPL / CLI 包装。** 增加依赖。整套东西的意义就是「文件 + 你现有 agent 客户端里的 slash command」。
