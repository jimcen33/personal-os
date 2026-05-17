# 综合 — 为什么合 Karpathy + Jeff Su

这个 template 是融合 Karpathy 的 LLM wiki schema 和 Jeff Su 的 Personal OS workflow，加上让它们作为一个工作品配合起来必须新发明的连接组织。

## 它们各自解决不了的错配

### 只有 Karpathy 的 wiki
强项：干净 schema、agent-readable、质量优先。
弱点：不告诉你 *什么时候* ingest、什么时候 lint、什么时候 query。Wiki 是目的地不是 workflow。

### 只有 Jeff Su 的 Personal OS
强项：真实 workflow、workstation、plain-language 接口、近期 context 缓存。
弱点：绑 Notion，schema 是隐式的（relation + property），对入门没操作纪律（捕获友好）。

### 合起来
Wiki 给 *what*（schema、类型化页面、治理）。OS 给 *when*（workstation、hot cache、routing）。结果是 agent 永远知道：
1. **这材料是什么**（type、layer、reliability）
2. **拿它怎么办**（ingest / queue / refuse / promote）
3. **你什么时候会需要它**（检索面被 `last-retrieved`、scope、active project 调过）

## 我们加的连接组织

Karpathy + Jeff Su 不完全在中间相遇。这个 template 加的部分：

### 1. Quality Gate（4 题）
Karpathy 讲质量门槛；Jeff 的模式捕获友好。4 题门是「除非通过否则不允许捕获」的形式化机制。没有它 wiki 会膨胀。

### 2. 三 zone ingest（Verbatim / Metadata / Decomposed）
Karpathy 的类型化页面干净；Jeff 的页面实用。Zone A 保原文（Karpathy 的「结构化数据重要」）；Zone B 加元数据（frontmatter、link）；Zone C 把方法论拆成可执行 SOP（我加的 — 「周一早上做什么」的拒绝启发式）。

### 3. Spotlight Rule
两个源头都没直接谈谄媚。Spotlight Rule 是带校准 loop 的 3 因子加权分。没它 LLM 驱动的 wiki 退化成「你可能觉得有意思」垃圾邮件。

### 4. Disposable Noise 拒收清单
显式。分类。Agent 拒收模型发布、融资八卦、「你需要知道的 10 件事」清单文。Karpathy 和 Jeff 都没有 — 这来自跑系统时发现 *总是* 穿门的东西。

### 5. Workstation 叠在根之上的组合
Jeff 有 workstation；Karpathy 基本没有。Karpathy 有类型化页面；Jeff 基本没有。组合起来 — 带自己类型化页面、voice 规则、workflow 的 workstation — 是整合招。

### 6. Multi-vault constellation 模式
两个源头都没谈一个 wiki 按敏感度分成多个时怎么办。这个 template 记录模式（和沙箱约束）让你能成长进去。

## 你拿到的哲学

合起来这套系统有立场：

- **拒收多于接收。** 大多数材料不值得留。
- **拆解不要总结。** 方法论变成 SOP。「这篇文章说什么」是错框架；「周一早上做什么」是对框架。
- **Agent 是 opinionated 的。** 它打分、拒绝、浮出 surprise。谄媚在结构上被防住。
- **写入永远要 human-in-the-loop。** 自动修是陷阱。提议；用户批准；然后写。
- **纯 Markdown 够了。** 没数据库、没插件、没 lock-in。

如果你不同意任何一条，fork。它们都是默认 on 的，是有理由的，但都可改。

## 这不是什么

- 不是「第二大脑」工具。第二大脑捕获；这个系统 *拒收*。
- 不是项目管理工具。任务在你从 `MEMORY.md` Active Projects 引用的独立系统里。
- 不是 chatbot wrapper。Agent 运营 wiki；wiki 不只是给 agent 的 context 倾倒处。
- 不是 Notion / Obsidian 竞品。是跑在纯文件之上的指令层 — 不替换你的编辑器。

## 什么时候这是错工具

不用这个 template 如果：

- 你要零结构 — 这是 opinionated 的，你会跟它打架。
- 你要不惜代价的捕获优先速度 — Quality Gate 故意慢下你。
- 你不针对笔记跑 LLM agent — 系统是 *给* agent 设计的。没 agent 你只是在手工维护结构。
- 你的知识是重结构化的（成千上万论文的文献综述、客户数据库）— 那些要真数据库不是 markdown wiki。

其他所有人 — solo founder、创作者、研究员、任何用 agent 跑个人知识系统的人 — 这就是我们造的。
