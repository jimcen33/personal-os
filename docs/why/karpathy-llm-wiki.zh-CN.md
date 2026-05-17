# 我们从 Karpathy 的 LLM Wiki 借了什么

Andrej Karpathy 公开（在 X 和各类对话里）讲过专门给 LLM 消费而不是人类阅读优先组织个人 wiki 的想法 — agent 读优先，人读其次。这个 template 把那个核心想法落地了。

## Karpathy 提的（我转述）

我理解的核心洞见：

1. **类型化页面。** 不用自由形式笔记，页面有声明的 type（concept、methodology、decision、reference）。Type 告诉 agent 怎么解析和使用页面。

2. **Agent-readable schema。** Wiki 有 `AGENTS.md` 风格的文件文档化 schema，给 agent 直接消费用，不需要读人面 README。

3. **质量门槛优先于数量。** Wiki 拒收低价值材料。小的高质量 wiki 跑赢大的平庸 wiki。

4. **Wiki 作为 agent 的底层。** Wiki 不是搜索目标 — 是 agent 知识和你知识共存的地方。Agent 读 wiki、写回去、长期改进它。

5. **跨页连接重要。** Wikilink、矛盾检测、孤儿查找是一等公民操作，不是事后补的。

## 这个 template 原样拿过来的

- **类型化页面。** Frontmatter `type:` 字段配小枚举（methodology / concept / decision / reference / decomposed / queue）。
- **`AGENTS.md`。** 可移植 wiki schema，设计成给任意 agent 的入口。
- **质量门槛。** 4 题 Quality Gate 在 ingest 时拒收低价值材料，不在检索时。
- **跨页连接。** `/wiki` 浮出缺失 link；`/lint` 找孤儿、broken link、矛盾；`/connections` 找跨想法模式。

## 我们扩展的

- **Quality Gate 被落实为 4 个具体打分题。** Karpathy 讲过质量门槛；这个 template 把它具体化、可测试化。
- **三 zone ingest 格式。** Verbatim / metadata / decomposed SOP。这是在类型化页面之上的封装。
- **Zone C 拆解。** 6 步 methodology-to-SOP 变换。新的。
- **Disposable Noise 拒收清单。** Agent 拒收的显式类别。新的。
- **Spotlight Rule。** 带校准 loop 的防谄媚机制。新的，我加的。

## 我们留下没做的

Karpathy 提过这个 template 不追求的几个方向：

- **自动 promote。** Karpathy 讲过 agent 自主改进 wiki。这个 template 在每次 promote 都要求 human-in-the-loop。我们认为摩擦就是重点。
- **重 structured-data 字段。** 这个 template 用轻量 YAML frontmatter，不用深结构化 schema。更好维护。
- **图数据库后端。** 这个 template 是纯 Markdown。图在 `[[wikilinks]]` 里隐式存在。

## 源头思考在哪里找

Karpathy 这话题的公开对话散在 X 帖和 podcast 露面里。可参考（搜索而不是直链，帖子会动）：

- X 上搜「@karpathy LLM wiki」
- Lex Fridman 播客有 Karpathy 的几期
- Karpathy 的 GitHub：github.com/karpathy

这个 template 是他想法的一个具体实例化 — 不是定义性解释。你的实现可能不同；他的版本可能不同。
