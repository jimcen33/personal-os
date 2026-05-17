---
last-updated: 2026-05-17
---

# Promotion Checklist

When a Queue item is ready to be promoted to a Permanent layer, run this checklist in a **fresh Cowork session**. The point of the fresh session is to score the item cold — without the bias of having just ingested it.

This file is read by the `/promote` skill.

---

## When to read this

- You're running `/promote` on a Queue item.
- The session has no prior ingest context for this item.
- If you're already deep in an ingest session: STOP. Open a new session. Then run `/promote`.

---

## Validator inputs

- The queue file path (e.g., `Notes/_Queue/some-thing-2026-04-12.md`)
- Its frontmatter (`gate-score`, `gate-failures`, `reliability`, `expires`)
- The current `entity-dictionary.md` (to check for duplicates)
- The destination layer the user is proposing

---

## Pre-promotion checks

### 1. Quality Gate re-run (cold)

Score each of the 4 questions 0 or 1 again, without referencing the ingest-time scores:

1. Will this still matter in 1 year?
2. Did it change my thinking, or just inform me?
3. Is it relevant to declared scope or current Active Projects?
4. Will I realistically retrieve this later?

**Pass threshold: 4/4 for Permanent. 3/4 → recommend EXTEND queue (another 30 days, often with a specific recompose action). <3 → recommend EXPIRE.**

### 2. Entity dictionary check

Does a canonical page already exist for the central entity?

- **Yes** → recommend MERGE into existing page (don't create a duplicate)
- **No, but similar pages exist** → flag the similar pages; user decides merge vs. create-with-cross-link
- **No** → safe to promote

### 3. Type validation

Is the proposed `type` correct?

- `methodology` → must have a clear 6-step Zone C SOP. If absent, recommend DECOMPOSE before promotion.
- `concept` → must have a precise one-paragraph definition. If muddled, recommend SPLIT into concept + example.
- `decision` → must have a stated outcome and reasoning. If missing reasoning, recommend EXTEND queue.
- `reference` → no Zone C needed.

### 4. Layer fit

Is the proposed layer correct?

- Drives a decision (money, health, legal)? → `LifeOS/`
- Tooling, code, product plan? → `Software/`
- Methodology, concept, original thinking? → `Knowledge/`
- A deliverable (draft, published)? → `Writing/`
- Writing craft methodology specifically? → `Writing/Knowledge/`

### 5. Cross-link discovery

Skim 3 nearest pages in the proposed layer. Surface any that should link to the new page. The agent does NOT auto-link — it proposes; user confirms.

---

## Output format

```
## /promote validator report — <queue-item-name>

**Recommendation:** PROMOTE | MERGE | EXTEND | EXPIRE | DECOMPOSE_FIRST | SPLIT

**Quality Gate cold-rescore:** N/4
- Q1: <pass/fail> — <reasoning>
- Q2: <pass/fail> — <reasoning>
- Q3: <pass/fail> — <reasoning>
- Q4: <pass/fail> — <reasoning>

**Entity check:** <duplicate found at [[page]] | no duplicates | similar pages: [[a]], [[b]]>

**Type check:** <correct | should be `<other-type>` because ...>

**Layer check:** <correct | should be `<other-layer>` because ...>

**Suggested cross-links:** [[a]], [[b]], [[c]]

**Notes:** <anything else the human should know>
```

---

## Common failure modes

- **Premature promotion.** Most ingested items don't deserve Permanent. Default to EXTEND if you're 50/50.
- **Duplicate creation.** Always check entity-dictionary. Two pages on the same concept is the most expensive mistake in this wiki.
- **Wrong layer.** A "how to invest" page that lives in `Knowledge/` instead of `LifeOS/Investing/` is silently broken — you won't find it during a money decision.
- **Type drift.** `methodology` without a clean Zone C SOP is essentially `reference` mis-labeled. Demand the SOP or downgrade the type.
