# 三个操作

Wiki 只有 3 个核心操作。其他东西都建在这 3 个之上。

## 1. Ingest

**目的：** 把原料移到正确的目的地层。

**触发：** `/ingest`、"process my notes"、"file this"、`Notes/Inbox/` 出现新内容。

**关键设计选择：**
- **Quality Gate 在入库前跑。** 低于 3/4 默认进 `_Queue/`。门是整个系统最 opinionated 的一块。
- **三 zone 格式。** Zone A 原文保留（≥40%）、Zone B 元数据、Zone C 拆解 SOP（条件性）。
- **先 dispatch 计划。** Agent 提案；用户批准；agent 写入。从不自动归档。
- **原子移动。** 源文件只在目标写入成功后才移到 `_archive/ingested/`。

完整 skill：[.claude/skills/ingest/SKILL.md](../../.claude/skills/ingest/SKILL.md)

## 2. Query（`/wiki <q>`）

**目的：** 搜 wiki、综合答案、为每个论点 cite、浮出缺失的 cross-link。

**触发：** `/wiki <question>`、"what do I know about X"、任何开放式问题。

**关键设计选择：**
- **每个论点都 cite** 带 `[[wikilinks]]`。用户必须能验证。
- **被引用页面 bump `last-retrieved`**。锚定 lint 的 Node Decay 检查。
- **浮出缺失 cross-link** 让用户批准并打补丁。Query 让 wiki 变更好。
- **Reliability 低的引用要显式标。**

完整 skill：[.claude/skills/wiki/SKILL.md](../../.claude/skills/wiki/SKILL.md)

## 3. Lint

**目的：** 一次健康检查一层。在腐烂扩散之前发现它。

**触发：** `/lint <layer>`、"lint my wiki"、"health check"。

**7 项检查：**
1. **Orphans** — 没有 inbound wikilink 的页面
2. **Stale** — `last-updated` >90 天
3. **Contradictions** — 跨页冲突（在页面内打 callout）
4. **Broken links** — 指向不存在页面的 wikilink
5. **Node decay** — `last-retrieved` >90 天
6. **Gaps** — 被引用 ≥3 次但没有 canonical 页的实体
7. **Zone C drift** — 拆解过的页漂回 summary 模式

**关键设计选择：**
- **不自动修。** 只报告。用户决定。
- **一次一层。** 全跑会产生一个看不懂的报告。
- **Contradiction callout 是唯一例外。** 这是唯一的写操作：在两个冲突页面里都打 `[!contradiction]` callout，因为矛盾在 *页面内* 浮出时最有用。

完整 skill：[.claude/skills/lint/SKILL.md](../../.claude/skills/lint/SKILL.md)

## 为什么是 3 个不是 5 个

初稿有 5 个操作：ingest、query、lint、promote、retire。Promote 被抽出来成独立 skill（要求 fresh session，值得单独文档）。Retire 被吸进 lint — 「归档什么」的决定通过看 lint 输出做出，不需要单独操作。

3 个是覆盖生命周期的最小集合。多于此 = 你在给子步骤起名字，不是在描述操作。

## 操作的时序

一周里典型节奏：
- **每天-ish：** `/ingest`（有新材料时）
- **每周：** `/queue-review`（快到期的项）
- **每周-ish：** `/wiki <q>`（有问题时）
- **每月：** `/lint <layer>`（每周一层，轮换）
- **按需：** `/queue-triage`（queue 涨大时）、`/promote`（fresh session 里）
- **session 结束：** `/hot-cache`（刷新近期上下文）
