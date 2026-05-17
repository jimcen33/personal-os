# 调整 Quality Gate

默认 Quality Gate（4 题，阈值 3）适合大多数 solo founder。有些用户想更严或更松。这里说怎么不打破系统地调。

## 诊断：调什么

### 症状：queue 堆满了从不被 promote 的项
阈值太低 — 通过 3/4 的项不值评估时间。动作：调到 4/4。

### 症状：你想留的东西被拒或被 queue
门太严，**或** 声明的 scope 太窄。审视：材料真的 off-scope，还是 scope 该扩？

### 症状：ingest 产出太多「我不确定」的 routing 请求
Agent 在犹豫，因为门题在你 context 里有歧义。动作：在 `CLAUDE.md` 里把题写得更针对你的 domain。

### 症状：噪音穿过门但从来没用
Disposable Noise 清单不完整。动作：把一直漏过的类别加上。

## 调阈值

根 `CLAUDE.md` 的 Quality Gate 段说：

> 任一答案 "no" → 进 `Notes/_Queue/`。Permanent 默认阈值：3。

改这个数字。然后更新 ingest skill 的 Step 5 逻辑：

```
gate-score < <新阈值> 的项 → 建议 Notes/_Queue/
gate-score >= <新阈值> 的项 → 建议 Permanent 层
```

常见调整：

| 阈值 | 用法 |
|---|---|
| 4 | 高纪律模式。低于 4/4 都进 queue。推荐 wiki 超过 ~100 页后用。 |
| 3 | 默认。允许 1 个「软失败」（通常 Q1 或 Q3）。 |
| 2 | 松模式。**不推荐** - 这是先收后筛，是这套系统设计来对抗的失败模式。 |

## 调题

你可以替换某题，如果某种过滤器在你 context 里更重要。例子：

### 顾问 / 研究员
Q4（「真的会去查」）改为：
> 我会在客户工作或研究输出里 cite 它吗？

### 创作者 / 作者
Q2（「改变思考」）改为：
> 这里有句话或洞见我会用进草稿吗？

### 投资人
Q1（「12 个月还重要」）改为：
> 它跨多个投资周期还重要，还是只这个周期？

**规则：** 保持 4 题。替换，不要加。4 题的简单度是重点。

## 调 Disposable Noise 清单

根 `CLAUDE.md` 的 Disposable Noise 清单直接拒收类别。把一直漏过的加进来。

常见随时间增加的项：

- 你看了标题就不需要看正文的简讯
- 「年度回顾」「最佳」清单文
- 没具体用例的通用 AI prompt 集
- 你已经发过的话题的复读教程

不要从清单里删类别，除非你很确定。每一条都代表一类之前伤过你 wiki 健康的内容。

## 调 Wiki Scope 声明

最重要、最被忽视的调整杠杆。

`CLAUDE.md` 的 Wiki Scope (declared) 段是决定「in scope」含义的那一句话。如果你发现门拒掉了有价值的材料：

1. **材料真的在 scope 内吗？** 是 → 你的 scope 句子太窄。
2. **材料在 scope 边缘吗？** 扩 scope 把它包进来 — 或者接受这东西不属于这里。

有 4 句 scope 声明的 wiki 比没 scope 声明的 wiki 过滤得好得多。

## 调门 vs. 调层

有时「问题」不在门 — 在目的地。如果一项通过门但落在错的层，问题在 layer dispatch，不在门。

快速诊断：
- 通过门但感觉「层不对」的项 → layer dispatch 问题。更新 ingest skill 的 routing 逻辑。
- 没通过门但感觉应该通过的项 → 门的问题。调阈值或题的措辞。
