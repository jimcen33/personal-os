# 架构

完整介绍 `personal-os` 是怎么组装起来的、每个零件为什么存在。想看「为什么是这些选择而不是别的」，读完这篇再看 [DECISIONS.zh-CN.md](DECISIONS.zh-CN.md)。

> 🇬🇧 English version: [ARCHITECTURE.md](ARCHITECTURE.md)

---

## 一段话的心智模型

这个 wiki 是一条 **5 层 pipeline**（Input → Knowledge → Skills → Action → Output），由 **3 个核心操作**（`/ingest`、`/wiki <q>`、`/lint`）驱动，受 **4 条治理规则**（Quality Gate、Spotlight Rule、Disposable Noise 拒收清单、明确声明的 Wiki Scope）约束，通过 **slash-command skills** 暴露接口，全部存为纯 Markdown 这样任何 agent 都能通过 `AGENTS.md` 接管运营。

---

## 五个层

每个文件只属于其中一个层。它们是从左到右的 pipeline。

| # | 层 | 文件夹 | 装什么 |
|---|---|---|---|
| 1 | Input | `Notes/` | 原始想法（`Inbox/`）、网页 clipping（`Clippings/`）、AI 对话存档（`Conversation/`）、30 天评估 queue（`_Queue/`） |
| 2 | Knowledge | `Knowledge/` | 持久的理解 — 方法论、框架、读书笔记、原创思考 |
| 3 | Skills | `Software/` | 工具、代码、产品/功能 plan、dev notes |
| 4 | Action | `LifeOS/` | 驱动决策的知识：投资、健康、保险、联系人、个人财务 |
| 5 | Output | `Writing/` | 交付物 — 草稿、已发表、脚本、加一个 `Writing HQ` workstation |

**为什么要分层？** 因为每层有不同的*流速*和*留存规则*。Notes 便宜可丢弃。Knowledge 昂贵且持久。Action 层要求精确（这些笔记会触发真金白银 / 健康 / 法律决策）。Output 是所有东西汇聚的地方。混在一起会稀释所有层。

---

## 三个操作

它们都是 skills（`.claude/skills/<name>/SKILL.md`），用 slash command 触发。

### `/ingest`
**工作：** 把 `Notes/` 里的原料按 Quality Gate 移到正确的目的地。

ingest 出来的页面是 **三个 zone** 结构：

- **Zone A — 原文保留。** Prompt template、step 序列、framework、数据表。≥40% 的源材料原貌。**绝不改写**。
- **Zone B — 元数据。** Cross-link、source URL、tags。在页面底部。
- **Zone C — 拆解 SOP。** 仅对方法论类源材料、且 reliability ≥ medium 时运行。6 个固定子段：
  1. 🩻 Core Question（核心问题）
  2. 🧽 Cut the Fluff（剔除废话）
  3. 📉 Plain-Language Translation（人话翻译）
  4. 🪜 Reverse-Engineered SOP（逆向工程出的 SOP）
  5. 🛡️ Failure Modes（失败模式）
  6. 🧐 Critical Review（批判性审视）

Zone C 是「一堆文章的 wiki」和「可操作知识的 wiki」之间的区别。

### `/wiki <question>`
**工作：** 搜索、综合、引用。

返回带 `[[wikilinks]]` 的答案，每个论点都有出处。在被引用页面上 bump `last-retrieved`（让 `/lint` 知道哪些页面还活着）。把应该建立但还没建立的 cross-link 浮出来等用户确认。

### `/lint <layer>`
**工作：** 一次健康检查一层。只报告，不自动修。

7 项检查：orphans（孤儿页）、stale（>90 天没更新）、contradictions（页面间冲突，会在两个页面里打 `[!contradiction]` callout）、broken links、node decay（`last-retrieved` >90 天）、gaps（被引用 ≥3 次但没有 canonical 页的实体）、Zone C drift（type:decomposed 页漂回 summary 模式）。

---

## 四条治理规则

### 1. Quality Gate
每个 ingest 项都必须回答：
1. 12 个月后这还重要吗？
2. 它改变了我的思考，还是只是让我知道了？
3. 它和我声明的 scope 或当前 Active Projects 相关吗？
4. 我以后真的会去查它吗？

得分：0–4。低于 3 → Queue（30 天到期）。3 及以上 → 候选 Permanent（仍需 dispatch 审批）。

### 2. Spotlight Rule
默认行为是 **沉默**。只有当 3 因子加权分超过 7.0 且 source 置信度 ≥ medium，才浮出高价值洞见。

打分（每项 1–10）：
- Leverage Type（× 0.4）— Naval 的杠杆栈：代码/媒体 > 资本 > 劳动力
- Specific Knowledge（× 0.3）— 锐化你的护城河 vs. 通用知识
- First-Principles Surprise（× 0.3）— 打破假设 vs. 确认已知

防疲劳：每个 session 最多 3 个 spotlight。每个 spotlight 都进 `00_Resources/spotlight-log.md`。30 条后根据结果分布重新校准阈值。

这条规则在结构上**防止谄媚** — Surprise 维度强迫 agent 给「打破用户既有模型」的洞见打分，纯赞同型 spotlight 凑不到 7.0。

### 3. Disposable Noise 拒收清单
除非用户明说「无论如何存下来」，agent **拒绝归档** 以下类别：
- AI 模型发布、benchmark、排行榜动态
- 融资新闻、估值、收购八卦
- 单条推「prompt hack」但没讲清底层原理
- 「X 领域的 10 个趋势」类清单文
- 简讯一句话就能总结的内容
- 第二大脑解释类文章 — 如果用户已经在跑同样的模式

### 4. 声明的 Wiki Scope
在 `CLAUDE.md` 里写 2–4 句话，声明这个 wiki 是**为什么** 而存在的。超出 scope 的材料默认进 queue 或丢弃。这是整个系统里最重要的一段文字 — 它管所有其他东西。

---

## Memory 层

三个文件加一个 hot cache：

- **`CLAUDE.md`** — 宪法。规则、偏好、routing map、治理。每个 session 都读。
- **`MEMORY.md`** — 持久事实。身份、活跃项目、决策、联系人、glossary。
- **`AGENTS.md`** — 可移植 wiki schema。让非 Claude 的 agent 只读这一个文件就能运营 wiki。
- **`00_Resources/hot.md`** — 会话缓存。约 250 字，告诉 agent 现在正在做什么。每个重型 session 结束时 `/hot-cache` 刷新。

---

## Workstations（工作站）

带自己规则、voice、resources 的子系统。每个是一个顶层文件夹，里面的 `CLAUDE.md` **叠在** 根 `CLAUDE.md` 之上 — 是叠加而不是替换。

这个 template 自带三个：

- **Writing HQ**（`Writing/Writing HQ/`）— 长文起草，9 步工作流（确认格式 → brainstorm → outline → SEO lock → draft → voice pass → humanizer pass → image prompts → 整体呈现）。
- **Email HQ**（`Email HQ/`）— 收件箱分流、回信起草、thread-aware response matching。
- **Tutorials**（`Tutorials/`）— 把技术诀窍变成非工程师友好的教程，配 `TUTORIAL_TEMPLATE.md` 和 `/tutorial <topic>` skill。

要加自己的 workstation，看 [docs/customization/adding-a-workstation.zh-CN.md](docs/customization/adding-a-workstation.zh-CN.md)。

---

## Skills

都在 `.claude/skills/<name>/SKILL.md`。每个 skill 有 frontmatter（`name`、`description`、`last-updated`）和正文，正文写清楚 trigger phrase、inputs、steps、outputs、failure modes。

Template 自带的核心 skills：

| Skill | 用途 |
|---|---|
| `ingest` | Quality-gated 捕获 + 三 zone 格式 + Zone C 拆解 |
| `queue-review` | 周度：下一周到期的 queue 项 |
| `queue-triage` | 批量：全 queue 扫描 + PROMOTE/EXTEND/SPLIT/EXPIRE 推荐 |
| `promote` | 独立验证器。**必须在 fresh session 跑。** |
| `lint` | 单层 7 项健康检查 |
| `wiki` | 搜索 + 综合 + 引用 + 浮出缺失 cross-link |
| `hot-cache` | session 结束时刷新 `00_Resources/hot.md` |
| `autoresearch` | 3 轮联网研究 + 填坑 |
| `voice-extract` | 从你的样本里抽取写作风格写入 `voice-principles.md` |
| `connections` | 找跨想法的连接，输出 writing brief 种子 |
| `new-info` | 验证当前信息的查询（拒绝幻觉式 UI 路径、过期事实） |
| `sync-tasks` | 检测 memory 文件和 scheduled task 之间的漂移 |
| `starter-session-audit` | session 结束扫一遍漏掉的纠正 / 偏好 |
| `humanizer` | 从草稿里除掉 AI 写作的味儿 |

---

## 数据流举例

### 「我读了一篇有意思的文章」
1. 粘进 `Notes/Inbox/`。
2. `/ingest` → agent 跑 Quality Gate。
3. 分数 < 3 → 进 `Notes/_Queue/`，30 天到期。完。
4. 分数 ≥ 3，是方法论类 → 进 `Knowledge/`，加 Zone C 拆解，写进 `00_Resources/index.md`，记入 `00_Resources/log.md`。

### 「我对 retention strategy 知道些什么？」
1. `/wiki retention strategy`
2. Agent 搜 `Knowledge/`，再 `Software/`，再 `Writing/Knowledge/`。
3. 综合答案，每个论点带 `[[wikilink]]`。
4. 每个被引用页面的 `last-retrieved` bump 到今天。
5. 注意到两个页面应该 link 却没 link → 问用户确认 → patch 该 link。

### 「我的 queue 越堆越多」
1. `/queue-triage`
2. Agent 读 `Notes/_Queue/` 里每个文件，解析 frontmatter，提取 TL;DR。
3. 每个 item 推荐 PROMOTE / EXTEND / SPLIT / EXPIRE 加一句理由。
4. 共享实体的 item 组队，方便批量 promote。
5. 生成 dispatch list 文件。用户去执行。

---

## 故意 **不在** 这里的

- **没有数据库。** 只有纯 Markdown。「schema」是 frontmatter + 文件夹约定。
- **没有全局搜索索引。** 文件足够小，grep + agent 驱动的搜索就够了。
- **没有自动修复。** `/lint` 只报告；用户去修。内容的话语权留给人。
- **不强制云端。** 本地磁盘、iCloud、Dropbox 都行 — 系统不绑路径。
- **不自动 promote。** Queue → Permanent 永远走 `/promote` + fresh session，这是设计。

---

## 继续看

- [DECISIONS.zh-CN.md](DECISIONS.zh-CN.md) — 为什么是这些选择，被拒掉的备选项有哪些
- [docs/architecture/five-layer-model.zh-CN.md](docs/architecture/five-layer-model.zh-CN.md)
- [docs/architecture/three-operations.zh-CN.md](docs/architecture/three-operations.zh-CN.md)
- [docs/architecture/quality-gate.zh-CN.md](docs/architecture/quality-gate.zh-CN.md)
- [docs/architecture/spotlight-rule.zh-CN.md](docs/architecture/spotlight-rule.zh-CN.md)
- [docs/architecture/workstations.zh-CN.md](docs/architecture/workstations.zh-CN.md)
- [docs/extending/multi-vault.zh-CN.md](docs/extending/multi-vault.zh-CN.md)
