---
name: connections
description: "Scan recent Queue items and new Permanent pages for non-obvious cross-idea connections. Four connection types: same principle in two domains (TYPE A), productive contradiction (TYPE B), three-note pattern (TYPE C), accidental question-answer pair (TYPE D). Outputs writing brief seeds. Triggers: /connections, find connections, what connects, brief seeds."
last-updated: "2026-05-17"
---

# Connections

Find the connections you didn't see. Output writing-brief seeds the user can later promote into full pieces.

## When to use

Weekly cadence before a writing session, or whenever the user says `/connections`, "find connections", "what connects", "brief seeds".

## Connection types

### TYPE A - Same principle, two domains
One mental model showing up in two unrelated areas. Example: "supply chain bottleneck theory" reappearing in "creative-process bottlenecks."

### TYPE B - Productive contradiction
Two pages making opposite claims that, when held together, reveal a real tension worth writing about.

### TYPE C - Three-note pattern
A pattern visible across 3+ separate pages that none of the individual pages name explicitly. Often the best brief seeds.

### TYPE D - Accidental question/answer
A question asked in one page, accidentally answered by a different page filed weeks later. The connection is invisible unless you specifically look.

## Scope

Limit to:
- Queue items added in the last 30 days
- Permanent pages added in the last 60 days

Looking back further produces noise (everything connects to everything if you squint).

## Step 1 - Build the candidate pool

List all pages matching the scope. For each, extract:
- Title
- Type
- Primary entities (from frontmatter tags + entity-dictionary)
- One-line summary

## Step 2 - Find candidates per type

### For TYPE A
Look for pages from different domain folders sharing an entity / mental model. Cross-domain matches are most valuable - same-domain matches are usually too obvious.

### For TYPE B
Look for pages tagged with the same entity but making opposite-direction claims. The entity dictionary helps - it records "claim direction" when known.

### For TYPE C
Cluster pages by overlapping entities. 3+ pages sharing 2+ entities suggests a pattern.

### For TYPE D
Search recent pages for question-shaped sentences. For each, search the rest of the candidate pool for answer-shaped sentences on the same entity.

## Step 3 - Score each candidate

For each candidate connection, score on the same rubric as the Spotlight Rule:
- Leverage Type (1-10)
- Specific Knowledge (1-10)
- First-Principles Surprise (1-10)

Weighted: (leverage * 0.4) + (specific * 0.3) + (surprise * 0.3). Cap output at the top 3-5 scoring above 7.0.

## Step 4 - Write brief seeds

For each surviving connection, write a seed file to `Notes/_Queue/connection-<slug>-<YYYY-MM-DD>.md`:

```yaml
---
title: <connection title - what the brief would argue>
type: connection-seed
connection-type: A | B | C | D
score: X.X
linked-pages: [[a]], [[b]], [[c]]
last-updated: YYYY-MM-DD
expires: YYYY-MM-DD  # +30 days
status: pending-review
---
```

Body:
- **Connection in one sentence** - the insight
- **Supporting pages** - what each contributes
- **Why this is interesting** - leverage / specific / surprise rationale
- **Possible brief angle** - one sentence the user could open a draft with

## Step 5 - Surface to user

Render the seeds as a table. The user picks which to keep, extend, or expire. Same lifecycle as any queue item.

## Failure modes

- **Forcing connections.** If the top score is below 7.0, output "no high-value connections this scan." Don't produce filler.
- **Same-domain TYPE A.** Two `Software/` pages on the same tool sharing an entity isn't a TYPE A connection - that's just normal cross-linking. TYPE A requires *different* domains.
- **Confusing co-occurrence with causation.** Two pages mentioning the same entity isn't a connection unless they say something *about* the entity that combines into a new claim.
