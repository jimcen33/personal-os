---
name: ingest
description: "Quality-gated capture with three-zone format and Zone C decomposition. Routes Notes/Inbox/ items into the right Permanent layer or into Notes/_Queue/ (30-day expiry). Refuses Disposable Noise. Adds 6-step Zone C SOP for methodology/how-to sources. Triggers: /ingest, ingest this, process my notes, file this, sort my inbox."
last-updated: "2026-05-17"
version: v6
---

# Ingest

The single funnel that moves raw material from `Notes/` into the right destination layer. Operationalizes the Quality Gate, the Disposable Noise refusal, and the three-zone format documented in `AGENTS.md`.

## When to use

User says `/ingest`, "ingest this", "process my notes", "file this", "sort my inbox", or drops new content into `Notes/Inbox/`, `Notes/Clippings/`, or `Notes/Conversation/`.

## Hard rules

- **Dispatch plan before action.** Always propose a table of (file -> target layer + reasoning + Decompose Y/N) and **wait for user approval** before writing.
- **Refuse Disposable Noise** unless user explicitly says "save this anyway."
- **Verbatim in Zone A.** Never paraphrase prompt templates, frameworks, step sequences, or structured data.
- **Zone C only when warranted.** Methodology/how-to sources at reliability >= medium that passed Quality Gate 4/4.
- **Atomic moves.** After successful filing, move the source to `_archive/ingested/` in the same operation.

## Step 1 - Read the rules

Before scanning, read:
1. `CLAUDE.md` - Wiki Scope (declared), Quality Gate, Disposable Noise list
2. `00_Resources/entity-dictionary.md` - to detect duplicate-concept candidates
3. `MEMORY.md` - Active Projects (for scope-fit on Q3)

## Step 2 - Scan inbox

List every new file in:
- `Notes/Inbox/`
- `Notes/Clippings/`
- `Notes/Conversation/`

For each, read enough to classify:
- Type (methodology / concept / decision / reference / raw-thought / disposable-noise)
- Reliability source (primary / trusted secondary / aggregator / unknown)
- Apparent scope-fit (matches declared Wiki Scope? matches Active Projects?)

## Step 3 - Refuse Disposable Noise

Per `CLAUDE.md` Disposable Noise list. For each refused item, report:

```
REFUSED: <filename> - <category>
Reason: <one line>
```

Unless user says "save this anyway," do not file these. Move them to `_archive/refused/` with a one-line reason in frontmatter.

## Step 4 - Run the 4-question Quality Gate

For each surviving item, score 0 or 1 on each:

1. Will this still matter in 1 year?
2. Did it change my thinking, or just inform me?
3. Is it relevant to declared scope or current Active Projects?
4. Will I realistically retrieve this later?

Record `gate-score` (0-4) and `gate-failures` (list of Q1-Q4 that failed).

## Step 5 - Propose dispatch table

Present a table:

| File | Gate score | Proposed layer | Proposed type | Decompose? | Reasoning |
|---|---|---|---|---|---|
| <name> | N/4 | `Knowledge/` etc. | methodology | Y | <one line> |

Items at gate-score < 3 -> propose `Notes/_Queue/` with 30-day expiry.
Items at gate-score >= 3 -> propose appropriate Permanent layer.

**STOP. Wait for user approval (full or per-item).**

## Step 6 - Write destination files

For each approved item, create the destination file with three zones:

### Frontmatter

```yaml
---
title: <human-readable>
type: <methodology | concept | decision | reference | decomposed | queue>
last-updated: YYYY-MM-DD
last-retrieved: YYYY-MM-DD
reliability: high | medium | low
gate-score: N
gate-failures: [<q1|q2|q3|q4>]
tags: [<topic>, <topic>]
---
```

For queue items, also add `expires: YYYY-MM-DD` (+30 days) and `status: pending-review`.

### Zone A - Verbatim

Preserve all prompt templates, step sequences, frameworks, benchmarks, structured data, and quotable passages. **>=40% of source substance.** Never paraphrase.

### Zone B - Metadata

Bottom of the page: source URL, date captured, cross-links, tags.

### Step 6.5 - Zone C decomposition (conditional)

Run **only when ALL** are true:
- gate-score = 4
- reliability >= medium
- type in {methodology, how-to, framework, prompt-template, workflow}

Zone C produces 6 mandatory subsections in this exact order:

#### Core Question
The single question the source actually answers. One sentence.

#### Cut the Fluff
The 80/20 - what 20% of the source carries 80% of the operational value? 3-5 bullets.

#### Plain-Language Translation
Restate the core method in plain English. No jargon from the source.

#### Reverse-Engineered SOP
The actual steps. Numbered. Specific. Each step starts with a verb. Each step has a "you should see X" success criterion.

#### Failure Modes
Top 3 ways this fails in practice. For each: how to detect, how to recover.

#### Critical Review
What's questionable about the source. Where the author overreaches. What evidence is missing. What you'd want to test before fully adopting.

**Refusal heuristic:** If you find yourself writing "this article discusses X," stop and ask "what does the reader DO Monday morning because of this?" If you can't answer that, the source isn't ready for Zone C - drop reliability one level or move to `_Queue/`.

If type is decomposed, set `type: decomposed` in frontmatter.

## Step 7 - Update index and log

- Append the new page link to `00_Resources/index.md`.
- Append one line to `00_Resources/log.md`: `YYYY-MM-DD | ingest | <page-path> | <layer> | decomposed: Y/N | <one-line reason>`.
- If a new entity was created, add it to `00_Resources/entity-dictionary.md`.

## Step 8 - Archive source

Move the source from `Notes/Inbox/` to `_archive/ingested/<YYYY-MM>/<original-filename>`. Atomic.

## Outputs

For the user: dispatch table with decisions, list of files written, queued, refused, and new index/log/dictionary entries.

## Failure modes

- **User says "ingest everything" without reviewing the dispatch table.** Push back - the dispatch table review is the human-in-the-loop on quality.
- **Source is paywalled / link-only.** Mark `reliability: unknown`, route to `_Queue/`, ask for a full extract.
- **Ambiguous layer assignment.** Default to `_Queue/` and surface the ambiguity.
- **Zone C feels forced.** Re-classify as `reference` or `concept` and skip Zone C.
