---
name: queue-triage
description: "Full-queue triage with auto-recommended actions. Scans every file in Notes/_Queue/, parses frontmatter, pulls a TL;DR from the body, and recommends PROMOTE / EXTEND / SPLIT / EXPIRE per item with reasoning. Generates dispatch lists and a promote handoff file. Use when the queue is large (>=20 items) and needs bulk cleanup. Does NOT promote in-session - promotion still requires a fresh session per promotion-checklist.md."
last-updated: "2026-05-17"
---

# Queue Triage

Compresses the manual queue cleanup into a review-and-approve flow. Works alongside `/queue-review` (which surfaces expiring items only) and `/promote` (which validates one item in a fresh session). This skill is the missing middle: full-queue triage with auto-recommendations and dispatch-list generation.

## When to use

User has captured a lot, the queue is large (>=20 items), and they want to clean it up in one pass. Triggers: `/queue-triage`, "triage my queue", "sort my queue", "recommend actions on the queue", "bulk queue cleanup".

Do **not** use this skill when fewer than 10 items are in the queue or when the user asks to promote a single item - those go to `/queue-review` or `/promote`.

## Hard rules

- **Never promote in-session.** Promotion requires the validator in a fresh Cowork session per `00_Resources/promotion-checklist.md`. This skill produces a handoff file; it does not promote.
- **Never auto-execute the expire batch without one explicit "proceed" from the user.** Dispatch plan before action is a root rule.
- **Atomic moves only.** When expiring, update frontmatter first, then move to archive in the same script run. No half-written files.

## Step 1 - Read the rules

Before scanning, read:
1. `CLAUDE.md` - Wiki Scope (declared), Quality Gate, Disposable Noise sections
2. `Notes/_Queue/README.md` if present - lifecycle, frontmatter schema, extension rules
3. `00_Resources/entity-dictionary.md` - to detect duplicate-concept candidates
4. `MEMORY.md` - Active Projects (to bias scope-fit Q3 toward current work)

## Step 2 - Scan every queue item

List every file in `Notes/_Queue/`. For each, parse frontmatter (`gate-failures`, `gate-score`, `reliability`, `status`, `expires`, `tags`) and pull a TL;DR from the body's first paragraph.

## Step 3 - Recommend per item

For each item, recommend ONE of:

- **PROMOTE** - gate-score now likely 4/4, distinct entity, methodology-shaped. Hand off to fresh-session validator.
- **EXTEND** - close but not quite. State which gate question needs more evidence; propose a 30-day extension.
- **SPLIT** - covers 2+ topics. Recommend splitting into N queue items, list the topics.
- **EXPIRE** - clearly won't promote. State which gate question(s) it permanently fails.

Each recommendation is one line of reasoning.

## Step 4 - Group connected items

Scan for shared entities across items. Group items that share an entity into "connected sets." A connected set of 3+ items often suggests one Permanent page absorbing them all.

## Step 5 - Present to the user

Render a card-style widget or table with one row per item:

```
| file | gate | recommendation | reason | connected-set |
|---|---|---|---|---|
```

Plus a separate "connected sets" summary.

**STOP. Wait for user approval. Approval can be all-PROMOTE, all-EXTEND, all-EXPIRE, per-item, or per-set.**

## Step 6 - Generate dispatch lists

After approval, write three text files in `Notes/_Queue/_dispatch-YYYY-MM-DD/`:

- `expire_list.txt` - one file path per line, ready for `xargs rm -f` (the user runs it themselves)
- `extend_list.txt` - one line per file: `<path>|<new-expires-date>|<reason>`
- `split_list.txt` - one block per file: original path + proposed split topics

## Step 7 - Write promote handoff

For items approved as PROMOTE, write a single file:

`Notes/_Queue/_promote-handoff-YYYY-MM-DD.md`

With one section per item:
- File path
- gate-score, reliability, type-hint
- Connected set (if any)
- Suggested cross-links
- Note to validator: "Run `/promote <path>` in a fresh session"

This file is the input to fresh-session `/promote` runs over the coming days.

## Outputs

- Triage report (widget + summary)
- `expire_list.txt`, `extend_list.txt`, `split_list.txt` in `_dispatch-YYYY-MM-DD/`
- `_promote-handoff-YYYY-MM-DD.md`

## Failure modes

- **User skips review.** This skill is high-stakes - missing reviews mean wrong-bulk-actions. If the user says "just do it," push back once explaining the stakes; if they confirm, proceed but log a note.
- **Connected-set hallucination.** If an entity link is weak (single mention, low semantic overlap), don't claim it's a connected set. Surface as "possibly related" instead.
- **Promote handoff with no items.** That's fine - just don't write a handoff file. Report "no promote candidates this triage."
