# Skills Reference

Every skill that ships with this template, in one table, with links to the canonical `SKILL.md` files.

| Skill | When to use | Trigger phrases |
|---|---|---|
| [ingest](../../.claude/skills/ingest/SKILL.md) | New material to file | `/ingest`, "process my notes", "file this" |
| [queue-review](../../.claude/skills/queue-review/SKILL.md) | Weekly check on items about to expire | `/queue-review`, "what's expiring" |
| [queue-triage](../../.claude/skills/queue-triage/SKILL.md) | Bulk cleanup when queue is large | `/queue-triage`, "triage my queue" |
| [promote](../../.claude/skills/promote/SKILL.md) | Validate a Queue item for Permanent (fresh session) | `/promote <path>`, "promote this" |
| [lint](../../.claude/skills/lint/SKILL.md) | Health-check a layer | `/lint <layer>`, "lint my wiki" |
| [wiki](../../.claude/skills/wiki/SKILL.md) | Answer a question from the wiki with citations | `/wiki <q>`, "what do I know about X" |
| [hot-cache](../../.claude/skills/hot-cache/SKILL.md) | Refresh session cache at end of work block | `/hot-cache`, "wrap up" |
| [voice-extract](../../.claude/skills/voice-extract/SKILL.md) | Detect writing patterns from samples | `/voice-extract`, "refresh my voice" |
| [autoresearch](../../.claude/skills/autoresearch/SKILL.md) | 3-round web research with gap-filling | `/autoresearch <topic>`, "research X" |
| [connections](../../.claude/skills/connections/SKILL.md) | Find cross-idea connections, output brief seeds | `/connections`, "what connects" |
| [new-info](../../.claude/skills/new-info/SKILL.md) | Verified current-info lookup | `/new-info <topic>`, "what's the latest on X" |
| [sync-tasks](../../.claude/skills/sync-tasks/SKILL.md) | Detect drift between memory files and scheduled tasks | `/sync-tasks`, "are my tasks up to date" |
| [starter-session-audit](../../.claude/skills/starter-session-audit/SKILL.md) | End-of-session: catch uncaptured corrections | `/session-audit`, "what did we miss" |
| [humanizer](../../.claude/skills/humanizer/SKILL.md) | Remove AI-writing tells from a draft | `/humanizer`, "humanize this" |
| [tutorial-generator](../../.claude/skills/tutorial-generator/SKILL.md) | Turn a technique into a non-engineer-friendly guide | `/tutorial <topic>`, "make a tutorial about X" |

## How skill loading works

When Cowork starts a session in this folder, it scans `.claude/skills/` and loads each skill's frontmatter. The `description` field is what the agent matches natural-language requests against. The body of the SKILL.md is loaded only when the skill is actually invoked.

This is why skill descriptions are dense - they need to disambiguate against every other skill plus generic LLM behavior.

## Adding your own skill

See [../customization/writing-your-own-skill.md](../customization/writing-your-own-skill.md) for the canonical template and contract.
