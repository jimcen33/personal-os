---
last-updated: 2026-05-17
---

# Cross-Project Bridge

How to point another project (a software repo, a different Cowork session, an external agent) at this wiki for read-only context.

---

## When to use this

- You have a code repo and want Cowork running in that repo to read this wiki for personal context.
- You have a company wiki / team wiki and want it to consume your voice principles.
- You're building an agent in a different framework and want it to operate on this wiki via `AGENTS.md`.

---

## Reading patterns

### Pattern 1: Cowork session in another folder reads this wiki

In the *other* project's `CLAUDE.md`, add:

```markdown
## External context

This project may read from my personal wiki at:
- `<absolute path to this wiki>` — for voice principles, methodologies, and durable knowledge
- `<absolute path>/00_Resources/voice-principles.md` — when drafting any user-facing content
```

Cowork follows file references when explicitly pointed at them. Do NOT write *into* the wiki from external sessions.

### Pattern 2: External agent reads `AGENTS.md`

Point the external agent at this wiki's `AGENTS.md`. That file is designed to be the entry point for any agent — it documents:
- The 5-layer schema
- The 3 operations (ingest/query/lint)
- The governance invariants
- The frontmatter contract

A capable agent can operate the wiki from `AGENTS.md` alone, no `CLAUDE.md` required.

### Pattern 3: RAG ingestion (read-only)

If you want an external RAG system to index this wiki:
- Index `Knowledge/`, `Software/`, `LifeOS/` (permanent layers)
- Exclude `Notes/` (raw, not yet quality-gated)
- Exclude `00_Resources/log-archive/` (historical, low retrieval value)
- Exclude any frontmatter field starting with `private:` (reserved for future sensitivity-flagging)

---

## Writing patterns

**Default rule: writes come from a Cowork session opened in THIS folder.** External sessions can read; they cannot write.

The one documented exception is the multi-vault routing pattern documented in `docs/extending/multi-vault.md` — there, a Cowork session in your *primary* vault can route auto-classified material into a *companion* vault. But the primary session is the one initiating the write, not the external one.

---

## Don't do this

- Don't share absolute paths in public docs (write `<your wiki path>` in examples).
- Don't grant write access to agents you don't audit — auto-writers will quickly corrupt frontmatter and break the lint invariants.
- Don't bypass the Quality Gate by writing directly to a permanent layer. If you want to add a page, drop it in `Notes/Inbox/` and `/ingest`.
