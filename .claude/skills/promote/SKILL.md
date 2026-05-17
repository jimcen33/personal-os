---
name: promote
description: "Independent validator for promoting a Queue item to Permanent. WARNING - must be run in a fresh Cowork session, not as a continuation. Scores the item cold against the Quality Gate, Typed Entity rules, and (for type:decomposed pages) Zone C completeness. Triggers: /promote, promote this queue item, validate this for permanent."
last-updated: "2026-05-17"
version: v2
---

# Promote (Independent Validator)

**STOP - IS THIS A FRESH SESSION?**

The Promotion operation requires an INDEPENDENT VALIDATOR. If you are continuing from an ingest or queue-review session, you must not run this. Tell the user to open a brand-new Cowork session, then re-trigger the promote skill.

If this IS a fresh session, proceed.

## Setup

You are running an INDEPENDENT VALIDATOR pass on a queued item. You have not seen the reasoning for queueing this item, and that is intentional. Score it cold.

Read these before scoring:
1. `AGENTS.md` (especially the Three Operations section on promotion)
2. `CLAUDE.md` Wiki Scope + Quality Gate sections
3. `00_Resources/promotion-checklist.md`
4. The queued item - the user must provide the path. If no path was given, ask for it and stop.

## Step 1 - Cold re-score the Quality Gate

For each of the 4 questions, give YES (1) / NO (0) with a one-sentence reason. Do NOT reference the ingest-time score - score cold.

1. Will this still matter in 1 year?
2. Did it change my thinking, or just inform me? (Look for evidence: corrections, new vocabulary, decisions, "I now believe X.")
3. Is it relevant to declared scope or current Active Projects?
4. Will I realistically retrieve this later?

## Step 2 - Entity dictionary check

Read `00_Resources/entity-dictionary.md`. Search for the central entity of the queue item.

- **Duplicate found** -> recommend MERGE into the canonical page. Don't promote a duplicate.
- **Similar pages found** -> list them. User decides MERGE vs CREATE-WITH-CROSS-LINKS.
- **No matches** -> safe to promote (if other checks pass).

## Step 3 - Type validation

Check the proposed `type`:

- **methodology** - must have a clear 6-step Zone C SOP. If absent, recommend DECOMPOSE_FIRST.
- **concept** - must have a precise one-paragraph definition. If muddled, recommend SPLIT.
- **decision** - must have a stated outcome and reasoning. If reasoning is missing, recommend EXTEND.
- **reference** - no Zone C required.

## Step 4 - Layer fit

Check the proposed destination layer:

- Drives a decision (money, health, legal)? -> `LifeOS/`
- Tooling, code, product plan? -> `Software/`
- Methodology, concept, original thinking? -> `Knowledge/`
- A deliverable? -> `Writing/`
- Writing craft methodology specifically? -> `Writing/Knowledge/`

If wrong, recommend the correct layer.

## Step 5 - Cross-link discovery

Skim 3 nearest pages in the proposed destination layer (same tag, similar title, adjacent in the entity dictionary). For each, decide: should the new page link to this one?

Don't auto-link. Propose. The user confirms before any patching.

## Step 6 - Output the recommendation

Render this exact format:

```
## /promote validator report - <queue-item-name>

**Recommendation:** PROMOTE | MERGE | EXTEND | EXPIRE | DECOMPOSE_FIRST | SPLIT

**Quality Gate cold-rescore:** N/4
- Q1: <pass/fail> - <reasoning>
- Q2: <pass/fail> - <reasoning>
- Q3: <pass/fail> - <reasoning>
- Q4: <pass/fail> - <reasoning>

**Entity check:** <duplicate found at [[page]] | no duplicates | similar pages: [[a]], [[b]]>

**Type check:** <correct | should be `<other-type>` because ...>

**Layer check:** <correct | should be `<other-layer>` because ...>

**Suggested cross-links:** [[a]], [[b]], [[c]]

**Notes:** <anything else>
```

## Step 7 - Wait for the user

Do not promote in this step. The output is a recommendation. The user reads it and decides to proceed (open ingest in a separate operation to file the page in its destination), or to act on the alternate recommendation.

## Failure modes

- **Not a fresh session.** Stop immediately. Tell the user to open a new session.
- **Path missing.** Ask for the queue item path. Don't guess.
- **You agree with the original ingest scoring before doing the cold rescore.** Bias. Do the cold scoring first, in isolation, then compare.
- **You miss a duplicate because the entity name varies.** Search aliases too - the entity dictionary records aliases for this reason.
