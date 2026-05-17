# Tuning the Quality Gate

The default Quality Gate (4 questions, threshold 3) works for most solo founders. Some users want it tighter or looser. Here's how to adjust without breaking the system.

## Diagnosis: what to change

### Symptom: queue is bloated with items that never get promoted
The threshold is too low - items pass at 3/4 that don't deserve evaluation time. Action: raise to 4/4.

### Symptom: things you want to keep are being refused or queued
The gate is too strict, OR your declared scope is too narrow. Audit: is the material genuinely off-scope, or does scope need to expand?

### Symptom: ingest produces too many "I'm unsure" routing requests
The agent is hesitating because the gate questions are ambiguous in your context. Action: rewrite question phrasing in `CLAUDE.md` to be more specific to your domain.

### Symptom: noise gets through but is never useful
The Disposable Noise list is incomplete. Action: add the categories that keep slipping through.

## Tuning the threshold

In root `CLAUDE.md`, the Quality Gate section says:

> If any answer is "no," route to `Notes/_Queue/`. Default threshold for Permanent: 3.

Change the threshold by editing this number. Then update the ingest skill's Step 5 logic:

```
Items at gate-score < <new-threshold> -> propose Notes/_Queue/
Items at gate-score >= <new-threshold> -> propose Permanent layer
```

Common adjustments:

| Threshold | When to use |
|---|---|
| 4 | High-discipline mode. Everything below 4/4 queues. Recommended after the wiki passes ~100 pages. |
| 3 | Default. Permits one "soft" failure (usually Q1 or Q3). |
| 2 | Loose mode. NOT recommended - this captures-then-prunes, which is the failure mode this system was designed against. |

## Tuning the questions

You can substitute questions if a different filter matters more in your context. Examples:

### For consultants / researchers
Q4 ("realistically retrieve") becomes:
> Will I cite this in client work or research output?

### For creators / writers
Q2 ("changed thinking") becomes:
> Is there a sentence or insight here I'd use in a draft?

### For investors
Q1 ("matter in 1 year") becomes:
> Will this matter across multiple investment cycles, or just this one?

**Rule:** keep four questions. Substitute; don't add. The simplicity of four is the point.

## Tuning the Disposable Noise list

In root `CLAUDE.md`, the Disposable Noise list refuses categories outright. Add categories that keep slipping past the gate.

Common additions over time:

- Newsletter editions where you read the subject line and didn't need to read the body
- "Year in review" / "best of" listicles
- Generic AI prompt collections without a specific use case
- Tutorial videos that recap something you already published

Don't remove categories from the list unless you're certain. Each one represents a category that previously cost you wiki health.

## Tuning the Wiki Scope declaration

The most important tuning lever, and the most overlooked.

`CLAUDE.md`'s Wiki Scope (declared) section is the single sentence that determines what "in scope" means. If you find the gate refusing valuable material:

1. **Is the material actually in scope?** If yes, your scope sentence is too narrow.
2. **Is the material on the edge of scope?** Expand the scope to include it - or accept that this material doesn't belong here.

A wiki with a 4-sentence scope statement filters better than one with no scope statement at all.

## Tuning the gate vs. tuning the layer

Sometimes the "problem" isn't the gate - it's the destination. If an item passes the gate but lands in the wrong layer, the issue is layer dispatch, not the gate.

Quick diagnostic:
- Items that pass the gate but feel "wrong" in their layer -> layer dispatch issue. Update the routing logic in the ingest skill.
- Items that fail the gate but feel like they should pass -> gate issue. Adjust threshold or question phrasing.
