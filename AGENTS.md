---
last-updated: 2026-05-17
---

# AGENTS.md — Portable Wiki Schema

This file documents the wiki's schema and operations in an agent-portable form. Any LLM agent — Claude, GPT, local model — can pick this up and run the wiki without reading `CLAUDE.md`. Where `CLAUDE.md` is the **constitution** (rules, preferences, voice), `AGENTS.md` is the **API** (data model, operations, invariants).

Inspired by the conventions emerging around `AGENTS.md` for agentic coding, adapted for a personal LLM wiki.

---

## Data Model

### Layers

The wiki has exactly five permanent layers, plus an input funnel.

| Layer | Folder | Purpose | Files have... |
|---|---|---|---|
| Input | `Notes/` | Raw capture, pre-dispatch | No required frontmatter |
| Knowledge | `Knowledge/` | Durable understanding | `type`, `last-updated`, `last-retrieved`, `reliability` |
| Skills | `Software/` | Tools, code, product plans | `type`, `last-updated`, `last-retrieved`, `reliability` |
| Action | `LifeOS/` | Decision-driving knowledge | `type`, `last-updated`, `last-retrieved`, `reliability` |
| Output | `Writing/` | Deliverables | `type`, `last-updated`, `status` |

### Page types

- `type: methodology` — A how-to. Always gets Zone C decomposition.
- `type: concept` — A definition or framework. Zone C optional.
- `type: decision` — A choice with reasoning. Frontmatter `outcome:` tracked.
- `type: reference` — Lookup material (terms, contacts, snippets). No Zone C.
- `type: decomposed` — A page with full Zone C SOP (the "active" version of methodology).
- `type: queue` — In `Notes/_Queue/` only. Has `expires:` field (default +30 days).
- `type: log` — Append-only timeline entries.

### Frontmatter contract

Required on every Permanent page:

```yaml
---
title: <human-readable>
type: <see types above>
last-updated: YYYY-MM-DD
last-retrieved: YYYY-MM-DD     # bumped on every successful /wiki query
reliability: high | medium | low
tags: [<topic>, <topic>]
---
```

Queue items additionally have:

```yaml
expires: YYYY-MM-DD              # 30 days after creation by default
gate-failures: [<q1|q2|q3|q4>]   # which Quality Gate questions it failed
gate-score: 0-4                  # how many gate questions it passed
status: pending-review | promoted | expired
```

---

## Three Operations

### 1. Ingest

**Input:** A file or paste in `Notes/Inbox/` (or `Notes/Clippings/`, `Notes/Conversation/`).
**Output:** Filed into one of the five layers, OR routed to `Notes/_Queue/` with expiry, OR refused as Disposable Noise.

Steps:
1. Refuse Disposable Noise. (See `CLAUDE.md` for the list.)
2. Read source. Determine `type`.
3. Run 4-question Quality Gate. Record `gate-score` and `gate-failures`.
4. If gate-score < 3 → route to `_Queue/` with 30-day expiry. Stop.
5. If gate-score ≥ 3 → propose a dispatch table (file → target layer, reasoning, Decompose Y/N).
6. **Wait for human approval.**
7. On approval, write the destination file in three zones:
   - **Zone A (verbatim):** Preserve all prompt templates, step sequences, frameworks, structured data. ≥40% of source substance. NEVER paraphrase.
   - **Zone B (metadata):** Cross-links, related pages, source URL, sits at the bottom.
   - **Zone C (decomposed SOP):** Only for `type: methodology` + reliability ≥ medium. Six mandatory subsections in this order:
     1. 🩻 Core Question
     2. 🧽 Cut the Fluff
     3. 📉 Plain-Language Translation
     4. 🪜 Reverse-Engineered SOP
     5. 🛡️ Failure Modes
     6. 🧐 Critical Review
8. Update `00_Resources/index.md` (topic map) and append one line to `00_Resources/log.md` with `decomposed: Y/N`.
9. Move source file to `_archive/ingested/`.

### 2. Query

**Input:** A natural-language question.
**Output:** A synthesized answer with citations to source pages.

Steps:
1. Identify the relevant layer(s).
2. Search file names, frontmatter tags, then full text.
3. For each cited page, bump `last-retrieved` to today.
4. Synthesize answer with `[[wikilinks]]` to source pages.
5. If two pages should link to each other but don't, surface the connection — ask human to confirm, then patch.
6. If reliability of a cited page is `low` or `medium`, flag it in the answer.

### 3. Lint

**Input:** A layer name (or "all").
**Output:** A report. Don't auto-fix.

Seven checks per layer:
1. **Orphans** — pages with zero inbound `[[wikilinks]]`.
2. **Stale** — `last-updated` > 90 days ago.
3. **Contradictions** — cross-page conflicts. Drop in-page `[!contradiction]` callouts.
4. **Broken links** — wikilinks pointing to nonexistent pages.
5. **Node decay** — `last-retrieved` > 90 days. Page might be dead weight.
6. **Gaps** — entities referenced ≥3× without a canonical page. Surface as `gap:` candidates for promotion.
7. **Zone C drift** — `type: decomposed` pages where the SOP section has drifted back into summary mode.

---

## Governance Invariants

The agent MUST NOT violate these without explicit user override:

1. **Default routing is Queue, not Permanent.** Promotion requires `/promote` validation in a fresh session.
2. **No auto-filing of ambiguous content.** Always propose dispatch plan, wait for approval.
3. **No paraphrase in Zone A.** Verbatim preservation of structured content.
4. **No edits to `_archive/`.** It's immutable history.
5. **No restating rules across files.** One rule, one canonical home.
6. **Refuse Disposable Noise unless user explicitly says "save this anyway".**
7. **Spotlight Rule fires only when score ≥ 7.0 AND confidence ≥ medium.** Default is silence.

---

## Skill Contract

Each skill in `.claude/skills/<name>/SKILL.md` has:

```yaml
---
name: <slug>
description: <when to use, ≤1024 chars>
last-updated: YYYY-MM-DD
---
```

Followed by Markdown body: trigger phrases, inputs, steps, outputs, failure modes.

Skills are slash-command-addressable: `/<name>` invokes them. Natural-language triggers also match via the description field.

---

## How to extend this schema

If you add a new layer or page type:
1. Update this file's "Layers" or "Page types" table.
2. Update `CLAUDE.md` Routing Map and Five-Layer Architecture table.
3. Update affected skills (especially `ingest`, `lint`, `wiki`).
4. Note the change in `00_Resources/log.md` with date.
