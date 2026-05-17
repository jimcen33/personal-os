# Skills 参考

Template 自带的所有 skill，一表说清，配 canonical `SKILL.md` 链接。

| Skill | 什么时候用 | 触发短语 |
|---|---|---|
| [ingest](../../.claude/skills/ingest/SKILL.md) | 有新材料要归档 | `/ingest`、"process my notes"、"file this" |
| [queue-review](../../.claude/skills/queue-review/SKILL.md) | 周度看要到期的项 | `/queue-review`、"what's expiring" |
| [queue-triage](../../.claude/skills/queue-triage/SKILL.md) | queue 大批量清理 | `/queue-triage`、"triage my queue" |
| [promote](../../.claude/skills/promote/SKILL.md) | 验证 queue 项进 Permanent（fresh session） | `/promote <path>`、"promote this" |
| [lint](../../.claude/skills/lint/SKILL.md) | 单层健康检查 | `/lint <layer>`、"lint my wiki" |
| [wiki](../../.claude/skills/wiki/SKILL.md) | 从 wiki 答问题带 citation | `/wiki <q>`、"what do I know about X" |
| [hot-cache](../../.claude/skills/hot-cache/SKILL.md) | 工作块结束刷新 session 缓存 | `/hot-cache`、"wrap up" |
| [voice-extract](../../.claude/skills/voice-extract/SKILL.md) | 从样本里抽取写作模式 | `/voice-extract`、"refresh my voice" |
| [autoresearch](../../.claude/skills/autoresearch/SKILL.md) | 3 轮联网研究 + 填坑 | `/autoresearch <topic>`、"research X" |
| [connections](../../.claude/skills/connections/SKILL.md) | 找跨想法连接，输出 brief 种子 | `/connections`、"what connects" |
| [new-info](../../.claude/skills/new-info/SKILL.md) | 验证当前信息的查询 | `/new-info <topic>`、"what's the latest on X" |
| [sync-tasks](../../.claude/skills/sync-tasks/SKILL.md) | 检测 memory 文件和 scheduled task 漂移 | `/sync-tasks`、"are my tasks up to date" |
| [starter-session-audit](../../.claude/skills/starter-session-audit/SKILL.md) | session 结束抓漏掉的纠正 | `/session-audit`、"what did we miss" |
| [humanizer](../../.claude/skills/humanizer/SKILL.md) | 从草稿里除 AI 写作的味儿 | `/humanizer`、"humanize this" |
| [tutorial-generator](../../.claude/skills/tutorial-generator/SKILL.md) | 把技术诀窍变成非工程师指南 | `/tutorial <topic>`、"make a tutorial about X" |

## Skill 加载怎么工作

Cowork 在这个文件夹启动 session 时扫描 `.claude/skills/`，加载每个 skill 的 frontmatter。`description` 字段是 agent 匹配自然语言请求用的。Skill 正文只在 skill 被实际调用时加载。

这就是为什么 skill description 写得密 — 它要在所有其他 skill 加通用 LLM 行为之间消歧。

## 加自己的 skill

完整模板和 contract 见 [../customization/writing-your-own-skill.zh-CN.md](../customization/writing-your-own-skill.zh-CN.md)。
