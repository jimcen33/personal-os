# 写你自己的 Skill

怎么加一个新 skill（slash command）到系统里。

## Skill 解剖

Skill 在 `.claude/skills/<name>/SKILL.md`。文件有 frontmatter 和正文。

### Frontmatter

```yaml
---
name: <slug>                   # 小写、连字符分
description: <什么时候用, <=1024 字符>
last-updated: YYYY-MM-DD
version: <可选 - 破坏性改动时 bump>
---
```

`description` 字段是**最重要**的。Cowork 用 description 去匹配自然语言请求，所以这字段需要：

- 一句话开场说这 skill 做什么
- 用户可能说的具体触发短语
- 不该用它的情况（如果存在类似 skill 容易混淆）

硬上限：1024 字符。提交前数一下：

```bash
awk '/^---$/{f=!f;next} f && /^description:/{sub(/^description: /,""); print | "wc -c"}' .claude/skills/<name>/SKILL.md
```

### 正文

正文段：

#### When to use
具体触发条件。用户说什么或做什么应该唤起这 skill？

#### Hard rules
Skill 永远不能违反的不变量。用这个编码「写之前等用户批准」「不要在 session 内 promote」之类的规则。

#### Steps
编号。每步：
- 动词开头
- 有清晰的「应该看到 X」成功标准
- 标出需要用户输入的 STOP 门

#### Outputs
Skill 产出什么？写文件？报告？widget？

#### Failure modes
3–5 种常见出错方式 + 怎么恢复。

## 设计原则

### 1. 一 skill 一操作
做两件不相关的事就拆。Skill 聚焦时更容易调试、测试、改进。

### 2. 先读规则
几乎每个 skill 都该以「先读这些 resource 文件：...」开始 — 给 agent 它需要的上下文。

### 3. STOP 门是真的
你标了「STOP - 等用户批准」，agent 就必须等。不能「这看起来明显，我继续」。这破坏用户信任。

### 4. 能报告就别行动
`/lint`、`/sync-tasks` 这种 skill 报告并提议。它们不自动修。用户决定。

### 5. 原子化破坏性动作
如果 skill 删或移文件，一步做完。不能半移。显式确认完成。

### 6. Cite AGENTS.md
对于 `AGENTS.md` 定义的操作（ingest/query/lint），引用它 — 不要重复记 schema。

## 例子：最小 skill

```markdown
---
name: weekly-review
description: "从最近 7 天的工作生成周回顾。从 MEMORY.md Recent Activity、hot.md、log 拉数据。输出：wins、blockers、open questions、下周优先级。触发：/weekly-review、weekly review、friday review。"
last-updated: 2026-05-17
---

# Weekly Review

## When to use
周五下午或用户说 /weekly-review 时。

## Hard rules
- 不包含 >7 天的项。
- 不编造 wins。如果一周被卡住，就说被卡住。

## Steps
1. 读 MEMORY.md Recent Activity 最近 7 天。
2. 读 00_Resources/hot.md 和 00_Resources/log.md（最近 7 天）。
3. 渲染 4 段：Wins、Blockers、Open Questions、Next Week's Priorities。
4. 让用户补漏。
5. 用户批准后保存到 Notes/Conversation/weekly-review-YYYY-MM-DD.md。

## Outputs
- 屏幕上的回顾
- 一个文件保存到 Notes/Conversation/

## Failure modes
- 空周（MEMORY/hot/log 里都没东西）。明说。别捏造。
- 用户要更细。建议 /retrospective 长版。
```

## 测试 skill

写完后在 3 个场景测：

1. **直接调用：** 输 `/<name>` — 加载并跑起来吗？
2. **自然语言匹配：** 输你 description 里 trigger 短语 — Cowork route 到这 skill 吗？
3. **失败 case：** 在前置条件不满足时触发 — 优雅失败吗？

任何一个坏掉，description 或 steps 还需要打磨。

## 分享 skill

如果 skill 通用到别人也能受益：

1. 按 `CONTRIBUTING.md` 提 PR。
2. 需要的话加 `docs/skills/<name>.md` 做深度解释。
3. 在 `docs/skills/README.md` 加一行。
4. 加到根 `CLAUDE.md` 的 skill 表。
