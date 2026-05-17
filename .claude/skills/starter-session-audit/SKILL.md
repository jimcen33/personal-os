---
name: starter-session-audit
description: "End-of-session audit. Scans the conversation for uncaptured corrections, preferences, and decisions, then proposes saving them to the right workspace file. Works with any Cowork workspace that has a CLAUDE.md and MEMORY.md in the root. Triggers: /session-audit, session audit, what did we miss, end of session check."
last-updated: "2026-05-17"
---

# Session Audit

End-of-session safety net. Scans the conversation for things that should have been saved but weren't.

## When to use

User says `/session-audit`, "session audit", "what did we miss", "end of session check". Also run automatically as part of `/hot-cache` workflows.

## Step 1 - Scan the conversation

Look back through the session for:

### Category A - Corrections to the agent's behavior
The user said things like "no, don't do it that way", "I prefer X over Y", "stop doing Z". These are *preferences* and may belong in `CLAUDE.md` (rules) or workstation `CLAUDE.md` (domain rules).

### Category B - New facts
The user mentioned new contacts, project status changes, decisions made, dates locked in, prices, account details. These belong in `MEMORY.md` or workstation `MEMORY.md`.

### Category C - Banned phrases / voice corrections
The user said "don't use the word X" or "I'd never write that way". These belong in `00_Resources/voice-principles.md`.

### Category D - New entity references
The user mentioned a new project / person / tool / methodology not yet in `entity-dictionary.md`.

### Category E - Things the user asked but you didn't resolve
Open questions you flagged but didn't answer. Add to `hot.md` "open questions".

## Step 2 - Propose dispatch

Render a table:

| What | Category | Proposed destination | Verbatim text or paraphrase |
|---|---|---|---|

For each row, the user can approve, edit, or skip.

**STOP. Wait for the user.**

## Step 3 - Apply approved items

On approval, write each item to its destination. For files the user is sensitive about (CLAUDE.md, voice-principles.md), show the diff before applying.

## Step 4 - Bump last-updated

For each file written to, bump its `last-updated` frontmatter to today.

## Failure modes

- **Auto-applying without approval.** Never. The session audit is gentle - the user should be in control of what gets persisted.
- **Over-eager rule-making.** Not every correction deserves a rule. If the user said "don't do X" once and the context was specific, don't promote it to a global rule. Ask: would I want this rule applied every session?
- **Missing implicit corrections.** Sometimes the correction is implicit - the user re-wrote your draft heavily, or chose option B when you recommended A. These count too, even if the user didn't say "you were wrong."
