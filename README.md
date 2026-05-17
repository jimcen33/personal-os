# personal-os

> An opinionated, file-first **LLM wiki + Cowork OS** for solo founders and creators.
> Inspired by [Andrej Karpathy's LLM Wiki](https://x.com/karpathy) idea and [Jeff Su's Personal OS](https://www.jeffsu.org/) architecture. Built, fused, and extended into a working system you can fork.

[🇨🇳 中文版 README](README.zh-CN.md) · [Architecture](ARCHITECTURE.md) · [Why these choices (DECISIONS.md)](DECISIONS.md)

---

## What this is

A plain-Markdown personal wiki, designed from day one to be run **by an LLM** (Claude Code / Cowork mode), not just searched by you. Drop a thought into `Notes/Inbox/`. Type `/ingest`. The agent runs a 4-question quality gate, decomposes methodology sources into actionable SOPs, files the result into the right layer, drops contradiction callouts when it spots conflicts, and updates a topic index. Every week, type `/queue-review` to triage what's about to expire. Every month, type `/lint` to find orphan pages, stale notes, and missing cross-links.

It's the system **as a working artifact** — folder structure, skills, governance rules, all here, ready to clone.

---

## Quickstart (60 seconds)

```bash
git clone https://github.com/{{your-handle}}/personal-os.git my-os
cd my-os

# 1. Open the folder in Cowork mode (or Claude Code)
# 2. Search & replace {{YOUR_NAME}}, {{YOUR_EMAIL}}, {{YOUR_DOMAIN}} across the repo
# 3. Edit the "Wiki Scope (declared)" section in CLAUDE.md
# 4. Start a session and type: /voice-extract  (or paste 5 of your sent emails)
# 5. Drop something in Notes/Inbox/ and type: /ingest
```

That's it. No build step, no database. Just files.

---

## The five-minute architecture

```
Input  ─►  Knowledge  ─►  Output
          ┌──────────┐
Notes/    │ Knowledge│   Writing/
          │ Software │
          │ LifeOS   │
          └──────────┘
```

Five folders, each with a job. Three operations (`/ingest`, `/wiki <q>`, `/lint`) move material between them. Four governance rules (Quality Gate, Spotlight Rule, Disposable Noise list, declared Wiki Scope) keep the wiki from rotting.

Read [ARCHITECTURE.md](ARCHITECTURE.md) for the full pipeline.

---

## What's in the box

| | |
|---|---|
| **Constitution** | `CLAUDE.md` (rules, routing, governance), `MEMORY.md` (durable facts), `AGENTS.md` (portable schema for any agent) |
| **Five layers** | `Notes/`, `Knowledge/`, `Software/`, `LifeOS/`, `Writing/` |
| **Workstations** | `Writing HQ/`, `Email HQ/`, `Tutorials/` — sub-systems with their own rules |
| **Resources** | `00_Resources/` — voice principles, index, log, prompt templates, spotlight log |
| **Skills** | ~17 slash-commands in `.claude/skills/`: `ingest`, `queue-review`, `queue-triage`, `promote`, `lint`, `wiki`, `hot-cache`, `autoresearch`, `voice-extract`, `connections`, `new-info`, `sync-tasks`, `humanizer`, and others |
| **Docs** | `ARCHITECTURE.md`, `DECISIONS.md`, `docs/` deep dives |

---

## Why this instead of Notion / Obsidian / Roam / a custom RAG

- **Notion / Roam / Obsidian** are *human-first* — they assume you'll read and link. This system assumes an LLM will read, link, decompose, lint, and surface.
- **Custom RAG** indexes everything as opaque chunks. This system *structures* knowledge into a typed, governed, decomposable form designed for LLM retrieval *and* human inspection.
- **Most "second brain" templates** capture forever. This one is built to **refuse low-quality material** and **expire what you don't promote**.

The trade-off: it's opinionated. Quality Gate, Spotlight Rule, Disposable Noise list, and Zone C decomposition are non-negotiable defaults. Fork it if you disagree — they're all in `CLAUDE.md`.

---

## Requirements

- macOS / Linux / Windows (no OS-specific code)
- A folder synced to wherever you want (iCloud, Dropbox, plain disk — your choice)
- [Claude Code](https://claude.com/claude-code) or Cowork mode (recommended). The system *can* run with other LLM clients, but skills (`/ingest`, `/lint`, etc.) assume Claude Code's skill-loading convention.

---

## Status

**v0.1 — usable, opinionated, evolving.**

I run a version of this every day. This is the public, sanitized, single-vault edition. The multi-vault extension (company-wiki + private-wiki + cross-vault routing) is documented in [docs/extending/multi-vault.md](docs/extending/multi-vault.md) for when you outgrow the single-vault setup.

---

## Contributing

PRs welcome. See [CONTRIBUTING.md](CONTRIBUTING.md). High-value contributions:

- Skills for new operations (e.g., `/weekly-review`, `/research-digest`)
- Adapters for other LLM clients (OpenAI Codex CLI, Cursor, local agents)
- Worked-example branches for different personas (consultant, researcher, designer)
- Translations of the README and docs

---

## License

MIT — see [LICENSE](LICENSE). Use it, fork it, ship it, sell what you build on top of it. Attribution appreciated but not required.

---

## Acknowledgments

- **Andrej Karpathy** — for articulating the LLM-wiki concept (typed pages, agent-readable schema, quality over quantity) in public conversations on X.
- **Jeff Su** — for the Cowork OS / Personal OS pattern (workstations, hot cache, plain-language interfaces).
- The Claude Code / Cowork team at Anthropic — for making skill-loadable agents practical.

This repo is the **synthesis** of those ideas into one usable artifact, with the parts I had to invent to make them fit together (Quality Gate, Spotlight Rule, Zone C decomposition, the governance ruleset). See [DECISIONS.md](DECISIONS.md) for what came from where and what's new.
