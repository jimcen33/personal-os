# 五层模型

> 深入讲 5 个层。高层总览见 [ARCHITECTURE.zh-CN.md](../../ARCHITECTURE.zh-CN.md)。

## 为什么是 5 个，不是 3 个或 7 个

3 层模型（Input → Knowledge → Output）干净，但失去了「理解」和「执行」之间的区别。「复利怎么运作」（Knowledge）和「我 2026 年 Roth IRA 的供款计划」（Action）这两个页面，留存规则、读者、更新节奏完全不同。合在一起两个都被稀释。

7 层模型（再加「短暂」「进行中」「归档」之类的子层）牺牲了简单度换取颗粒度。「这玩意儿放哪？」应该是个秒答的问题。

5 层是 solo founder / 创作者的甜蜜点。

## 逐层讲

### 1. Input — `Notes/`

所有新材料的唯一漏斗。4 个子文件夹：
- `Inbox/` — 原始想法、剪贴板倾倒、语音备忘转录
- `Clippings/` — 网页文章、截图、PDF
- `Conversation/` — 值得留的 AI 对话存档
- `_Queue/` — 通过初筛但等待 promotion 的项（30 天计时器）

**留存规则：** Inbox/Clippings/Conversation 被 `/ingest` 处理后清空。`_Queue/` 项 30 天后自动到期（除非被 promote）。

**读者：** 只有你和 agent。这一层是私有工作空间。

### 2. Knowledge — `Knowledge/`

持久的理解。方法论、框架、读书笔记、原创思考。变化最慢的一层。

**留存规则：** 页面不到期。陈旧页面（`last-updated` >90 天对常青内容没事；看 `last-retrieved` 才是相关性信号）。

**读者：** 未来的你，可能还有你授权的 agent。

### 3. Skills — `Software/`

工具、代码、产品/功能 plan、dev notes。所有技术或产品相关的东西。

**留存规则：** 工具变得快 — 90 天没更新就标 stale。UI 类教程做最严格的新鲜度审查。

**读者：** 未来需要某个具体 how-to 的你。可能合作者，如果你 cross-link 到了共享 repo。

### 4. Action — `LifeOS/`

驱动现实决策的知识：钱、健康、联系人、保险、法律。风险最高的一层。

**留存规则：** 这些页面在 promote 之前要交叉验证。陈旧规则很危险 — 90 天就标，要求重新验证。

**读者：** 主要是你。Agent 可能用这些信息辅助决策，但永远要 cite + 标 reliability。

### 5. Output — `Writing/`

草稿（`Drafts/`）、已发表（`Published/`）、写作方法论（`Knowledge/` — 那是 *meta* 写作知识），加一个 `Writing HQ` workstation。

**留存规则：** 发出后草稿移到 `Published/`。`Published/` 不到期。

**读者：** 全世界（如果发出去）或者未来的你（如果留私下）。

## 东西放哪 — dispatch 逻辑

ingest 不确定时会问。粗略启发式：

| 问题 | 如果 yes... |
|---|---|
| 触发钱 / 健康 / 法律 / 联系人决策？ | `LifeOS/` |
| 关于工具、语言、framework、codebase？ | `Software/` |
| 方法论、概念、模式？ | `Knowledge/` |
| 是个交付物或写作品？ | `Writing/` |
| 专门是写作 craft 的方法论？ | `Writing/Knowledge/` |
| 都不是但值得留？ | `Notes/_Queue/` |
| Disposable noise？ | 拒收 |

## 子层扩展

如果某层大到不好导航，按主题分子文件夹。`LifeOS/Investing/`、`LifeOS/Health/` 等都预创了。`Knowledge/` 和 `Software/` 初始是平的 — 单领域到了约 30 页再分子文件夹。

**不要预先分子文件夹。** 空的子文件夹会暗示「你应该往这归档」，把 ingest 推向过度归档。

## 反模式

- **把 `Notes/` 当停车场。** `Notes/` 是漏斗，不是目的地。里面的项应该在向目的地移动或者正在到期。
- **`Knowledge/` 当 clipping 文件夹。** 一个页面如果读起来像文章而不是原创综合，就不该在 Knowledge。要么拆成 Zone C SOP 格式，要么降级到 reference type，要么进 queue。
- **`Software/` 装你再不会用的软件决策。** Lint 的 Node Decay 检查会逮到。
- **`LifeOS/` 装研究而不是决策。** 决定性的问题：「如果这是错的，我会损失钱 / 伤害健康 / 触犯法律吗？」是 → LifeOS。不是 → 大概是 Knowledge。
