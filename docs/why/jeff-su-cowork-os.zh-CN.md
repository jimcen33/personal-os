# 我们从 Jeff Su 的 Personal OS / Cowork OS 借了什么

Jeff Su（jeffsu.org）通过 YouTube 和 Notion 模板普及了「Personal OS」框架：一个统一的操作系统给你的工作和生活，处理跨多个 context 的捕获、处理、输出。

他的模式本质上是这个 template 的 *workflow* 层，叠在 Karpathy wiki schema 之上。

## Jeff 提的（我转述）

我理解的模式：

1. **一个系统管所有。** 不要分开的「知识管理」和「任务管理」和「邮件」工具。一个系统 route 工作到正确的 sub-context。

2. **Workstation。** OS 内的子系统，有自己的规则、voice、workflow。Email 是一个 workstation。Writing 是另一个。Personal Finance 是另一个。

3. **Hot cache。** 每个 session 启动就加载的近期 context，系统不用每次问「我们刚才在做啥？」

4. **5 层 pipeline。** Input → Knowledge → Skills → Action → Output。每层不同留存规则、不同受众。

5. **Plain-language 接口。** 用户用人话不用 config。「Process my notes」就行；SQL 不行。

## 这个 template 原样拿过来的

- **Workstation。** 有自己 `CLAUDE.md` 叠在根宪法上的文件夹。我们出厂 Writing HQ、Email HQ、Tutorials。
- **Hot cache。** `00_Resources/hot.md` 由 `/hot-cache` 在 session 结束时刷新。
- **5 层 pipeline。** `Notes/` → `Knowledge/` → `Software/` → `LifeOS/` → `Writing/`。
- **Plain-language 接口。** Skill 用 slash command 和自然语言短语而不是 config 文件触发。

## 我们扩展的

- **Workstation 模式被形式化** 加 3 个必需文件（`CLAUDE.md`、`MEMORY.md`、`<Workstation> Resources/`）。Jeff 的模式更松散。
- **Memory 层被切分** 成根 `MEMORY.md`（持久事实）+ workstation `MEMORY.md`（域特定）+ `hot.md`（近期 context）。更清晰的分离。
- **分层 CLAUDE.md。** Workstation 规则 *叠在* 根规则之上，有显式 override 语义。这是软件工程模式（组合优于替换）套用在指令文件上。

## 我们做得不一样的

- **入门就 Quality Gate。** Jeff 的模式更欢迎捕获。我们门口更严 — Quality Gate 在材料进来前拒。
- **Karpathy 风格 schema。** Jeff 的模板 Notion-friendly（database 属性、view、relation）。我们用纯 Markdown + frontmatter。不同取舍：少查询能力，多可移植性。
- **Spotlight Rule 和 Disposable Noise 清单。** 这些在 Jeff 模式里没有。它们反映更 agentic 的姿态：系统主动 gatekeep，不只 route。

## 源头思考在哪里找

Jeff Su 的内容（jeffsu.org、YouTube、Twitter）：
- Personal OS Notion 模板
- 「How I use Notion」视频系列
- 「Cowork OS」框架（较新）

他的模板是 Notion 优先；这个 template 是 Markdown 优先。不同 stack，相同架构直觉。

## 为什么合起来

Karpathy 的 wiki 给你 *schema* — agent 友好系统里知识长什么样。Jeff 的 OS 给你 *workflow* — 知识在你工作时怎么在系统里流动。

Wiki 没有 workflow 会卡在「我有好结构但从不用」。Workflow 没有 wiki 会卡在「我全捕获但什么也找不到」。合起来才是这个 template 形式化的底层。
