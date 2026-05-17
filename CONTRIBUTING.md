# Contributing to personal-os

Thanks for being interested. This is an opinionated system, so contributions that **sharpen the opinions** are usually more useful than contributions that soften them.

---

## What I'm most interested in

- **New skills** that fill gaps in the operation pipeline (e.g., `/weekly-review`, `/research-digest`, `/calibrate-spotlight`)
- **Adapters for other LLM clients** so users not on Claude Code can run the system (OpenAI Codex CLI, Cursor, local models with skill-loading shims)
- **Worked-example branches** for different personas (independent researcher, consultant, designer, technical writer)
- **Translations** of `README.md`, `ARCHITECTURE.md`, `DECISIONS.md`, key docs
- **Edge cases that break the current rules** — open an issue with the case; I'd rather fix the rule than work around it

## What I'm less interested in

- Cosmetic README changes
- Adding "support for X tool" that requires a heavy plugin or build step (the system stays plain Markdown)
- Removing opinionation. The Quality Gate, Spotlight Rule, Disposable Noise list, and Zone C decomposition are load-bearing. If you want to fork and remove them, please do — but that's not a PR I'll merge.

---

## Process

### Bug or unclear doc

1. Open an issue with the [bug template](.github/ISSUE_TEMPLATE/bug.md).
2. If you have a fix, open a PR referencing the issue.

### New skill or substantial feature

1. Open an issue with the [feature template](.github/ISSUE_TEMPLATE/feature.md) **before** writing code.
2. Wait for a thumbs-up on scope. This saves you wasted work.
3. PR with the skill at `.claude/skills/<name>/SKILL.md`, a docs page at `docs/skills/<name>.md`, and an entry in the `CLAUDE.md` skills table.

### Translation

1. Fork.
2. Translate `README.md` → `README.<lang>.md`. If you can, also `ARCHITECTURE.md` and `DECISIONS.md`.
3. Open PR. Keep the link to original docs in case of drift.

---

## Style for documentation

- **Default to prose.** Bullets and tables only when content is genuinely list-shaped.
- **Show the trade-off.** Every opinionated rule should explain what it costs.
- **No marketing voice.** This isn't a product page.
- **Code blocks for paths and commands; backticks for filenames and skill names.**
- **English is the canonical language for docs.** Other translations follow.

---

## Style for skills

Skills live in `.claude/skills/<name>/SKILL.md` with this frontmatter:

```yaml
---
name: <slug, no spaces, lowercase>
description: <when to use, ≤1024 chars, includes trigger phrases>
last-updated: YYYY-MM-DD
---
```

The body should have:

- **Triggers** — slash command + natural-language phrases that should match
- **Inputs** — what the skill expects (files, arguments, state)
- **Steps** — numbered, explicit, with stopping points where the agent must wait for user approval
- **Outputs** — what gets written, where, with what frontmatter
- **Failure modes** — what to do when the input is wrong / missing / ambiguous

Description field is hard-capped at 1024 characters. Check before committing:

```bash
awk '/^---$/{f=!f;next} f && /^description:/{sub(/^description: /,""); print | "wc -c"}' .claude/skills/<name>/SKILL.md
```

---

## Code of conduct

Be the contributor you'd want to receive a PR from. Disagree on the technical merits; don't make it personal.

If you wouldn't say it in a meeting with your favorite senior colleague, don't say it in an issue thread.

---

## Maintainer

This repo is actively maintained. Expect a response within a week. If something is on fire (broken docs, dangerous instruction in a skill), open an issue and tag it `urgent` — I'll triage faster.
