# personal-os

> 给独立创始人和创作者的、文件优先的 **LLM 个人 wiki + Cowork OS** template。
> 灵感来自 [Andrej Karpathy 的 LLM Wiki](https://x.com/karpathy) 思路 和 [Jeff Su 的 Personal OS](https://www.jeffsu.org/) 架构。在它们之上做了融合和扩展。

[🇬🇧 English README](README.md) · [架构 ARCHITECTURE](ARCHITECTURE.zh-CN.md) · [设计决策 DECISIONS](DECISIONS.zh-CN.md)

---

## 这是什么

一个纯 Markdown 的个人 wiki，从第一天起就是为了让 **LLM (Claude Code / Cowork) 来运营** 而不是仅供你搜索。把一个想法丢进 `Notes/Inbox/`。输入 `/ingest`。Agent 跑一遍 4 题质量门，把方法论类来源拆解成可执行 SOP，归到正确的层，发现矛盾就在页面里打 callout，更新主题索引。每周输入 `/queue-review` 看哪些要到期。每月输入 `/lint` 找孤儿页、过期页、缺失的双向链接。

它是**一个跑得起来的工作系统** — 文件夹结构、skills、治理规则，都在这里，可以直接 clone。

---

## 60 秒快速上手

```bash
git clone https://github.com/{{your-handle}}/personal-os.git my-os
cd my-os

# 1. 在 Cowork 模式（或 Claude Code）里打开这个文件夹
# 2. 全仓搜索替换 {{YOUR_NAME}}、{{YOUR_EMAIL}}、{{YOUR_DOMAIN}}
# 3. 编辑 CLAUDE.md 里 "Wiki Scope (declared)" 部分
# 4. 启动 session，输入：/voice-extract（或贴 5 封你的发件箱邮件）
# 5. 往 Notes/Inbox/ 里丢点东西，输入：/ingest
```

完。没有 build 步骤，没有数据库。只是文件。

---

## 5 分钟看懂架构

```
Input  ─►  Knowledge  ─►  Output
          ┌──────────┐
Notes/    │ Knowledge│   Writing/
          │ Software │
          │ LifeOS   │
          └──────────┘
```

5 个文件夹，每个有自己的职责。3 个操作（`/ingest`、`/wiki <q>`、`/lint`）在文件夹之间移动内容。4 条治理规则（Quality Gate、Spotlight Rule、Disposable Noise 拒收清单、声明的 Wiki Scope）防止 wiki 腐烂。

完整 pipeline 见 [ARCHITECTURE.zh-CN.md](ARCHITECTURE.zh-CN.md)。

---

## 盒子里有什么

| | |
|---|---|
| **宪法** | `CLAUDE.md`（规则、routing、治理）、`MEMORY.md`（持久事实）、`AGENTS.md`（任意 agent 的可移植 schema） |
| **5 层** | `Notes/`、`Knowledge/`、`Software/`、`LifeOS/`、`Writing/` |
| **Workstation** | `Writing HQ/`、`Email HQ/`、`Tutorials/` — 带自己规则的子系统 |
| **资源** | `00_Resources/` — voice 原则、index、log、prompt 模板、spotlight 日志 |
| **Skills** | `.claude/skills/` 里约 15 个 slash command：`ingest`、`queue-review`、`queue-triage`、`promote`、`lint`、`wiki`、`hot-cache`、`autoresearch`、`voice-extract`、`connections`、`new-info`、`sync-tasks`、`humanizer` 等 |
| **文档** | `ARCHITECTURE.md`、`DECISIONS.md`、`docs/` 深入篇 |

---

## 为什么用这个，不用 Notion / Obsidian / Roam / 自定义 RAG

- **Notion / Roam / Obsidian** 是 *人优先* — 假设你会去读和 link。这套系统假设 LLM 会去读、link、拆解、lint、surface。
- **自定义 RAG** 把所有东西索引成不透明 chunk。这套系统把知识 *结构化* 成类型化、治理化、可拆解的形式，给 LLM 检索 *和* 人类审视一起用。
- **大多数「第二大脑」模板** 永远捕获。这套是为了 **拒收低质量材料** 和 **过期没 promote 的东西** 建的。

代价：opinionated。Quality Gate、Spotlight Rule、Disposable Noise 清单、Zone C 拆解都是不可商量的默认。不同意就 fork — 都在 `CLAUDE.md` 里。

---

## 要求

- macOS / Linux / Windows（无 OS 特定代码）
- 同步到任何地方的文件夹（iCloud、Dropbox、纯本地磁盘 — 你选）
- [Claude Code](https://claude.com/claude-code) 或 Cowork 模式（推荐）。系统 *可以* 跟其他 LLM 客户端跑，但 skill（`/ingest`、`/lint` 等）假设 Claude Code 的 skill 加载约定。

---

## 状态

**v0.1 — 可用，opinionated，演进中。**

我每天跑这个的某个版本。这是公开、脱敏、单 vault 版本。多 vault 扩展（company-wiki + private-wiki + 跨 vault routing）记在 [docs/extending/multi-vault.zh-CN.md](docs/extending/multi-vault.zh-CN.md)，等你长大了用得到。

---

## 贡献

PR 欢迎。见 [CONTRIBUTING.md](CONTRIBUTING.md)。高价值贡献：

- 新操作的 skill（比如 `/weekly-review`、`/research-digest`）
- 其他 LLM 客户端的适配器（OpenAI Codex CLI、Cursor、本地 agent）
- 不同人物的 worked-example 分支（顾问、研究员、设计师）
- README 和文档的翻译

---

## 协议

MIT — 见 [LICENSE](LICENSE)。用它、fork 它、ship 它、在它之上做商业产品都可以。署名感激但不强求。

---

## 致谢

- **Andrej Karpathy** — 在 X 上的公开对话里阐述 LLM-wiki 概念（类型化页面、agent-readable schema、质量优先）。
- **Jeff Su** — Cowork OS / Personal OS 模式（workstation、hot cache、plain-language 接口）。
- Anthropic 的 Claude Code / Cowork 团队 — 让 skill-loadable agent 变得实用。

这个 repo 是把上面那些想法**融合**成一个可用工作品，加上我为了让它们配合工作必须新发明的部分（Quality Gate、Spotlight Rule、Zone C 拆解、治理规则集）。各部分来源见 [DECISIONS.zh-CN.md](DECISIONS.zh-CN.md)。
