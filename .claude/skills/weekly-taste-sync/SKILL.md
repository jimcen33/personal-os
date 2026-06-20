---
name: weekly-taste-sync
description: "Cross-session behavior-taste aggregator. Scans the last 7 days of session transcripts, extracts candidate rule patches across 5 buckets (Procedural / Trigger / Execution / Outcome / Voice), runs the Skeptic plus one mandatory adversarial candidate, and presents patches for explicit approval. NEVER auto-applies. Run once a week. Calibrates via rollback rate in 00_Resources/taste-sync-log.md. Triggers: /weekly-taste-sync, sync my taste, weekly taste review, taste drift check."
last-updated: "2026-06-20"
---

# Weekly Taste Sync

The controlled, once-a-week path for behavior-taste changes to reach `CLAUDE.md` / `MEMORY.md` / `voice-principles.md`. The root rule is: **do NOT auto-mutate those files mid-session.** Daily taste loops over-fit to mood. This skill batches a week of signal, filters it adversarially, and lets you approve explicitly.

## When to use

User says `/weekly-taste-sync`, "sync my taste", "weekly taste review", or "taste drift check." Intended as a once-a-week ritual (e.g. Sunday evening). Companion to `/session-audit` (single-session).

## Hard rules

- **Never auto-apply.** Every patch is presented; the user approves or rejects each one.
- **8-patch ceiling.** Surface at most 8 candidate patches per sync. More than that and the loop is over-fitting — tighten the score threshold.
- **One mandatory adversarial candidate.** At least one patch each sync must argue *against* a recent pattern (e.g. "you've been doing X; here's the case for stopping"). This prevents the sync from only ratifying habits.
- **Skeptic pass before presenting.** Run the candidate patch set through `/skeptic` and drop any patch whose top failure mode is unaddressed.
- **60-day decay flag.** Any rule added by a past sync that hasn't been re-confirmed or relied on in 60 days gets flagged for possible removal.

## Workflow

1. **Scan** the last 7 days of session activity (transcripts, `run-log.jsonl`, corrections the user made in-session).
2. **Extract** candidate patches into 5 buckets:
   - **Procedural** — a step order or workflow change.
   - **Trigger** — a new phrase that should route to a skill/behavior.
   - **Execution** — how a task should be carried out (format, depth, tools).
   - **Outcome** — a standard for what "done" means.
   - **Voice** — a writing/tone preference (these route to `voice-principles.md`).
3. **Score** each candidate (impact × frequency). Keep the top 8.
4. **Add** the mandatory adversarial candidate.
5. **Skeptic pass** — run the set through `/skeptic`; drop the weak ones.
6. **Present** the surviving patches as a table: `patch | target file | bucket | rationale | approve?`.
7. **Apply only the approved patches.** Update the target files and bump their `last-updated`.
8. **Log** one entry to `00_Resources/taste-sync-log.md` (patches proposed/approved, the adversarial candidate, rollbacks since last sync).
9. **Append** one line to `00_Resources/run-log.jsonl`.

## Calibration

At month boundaries, read `taste-sync-log.md` and compute the rollback rate (approved patches you later reverted). High rollback → raise the score threshold. Low rollback → the threshold is healthy.

## Failure modes

- **Ratifying mood.** If every patch agrees with what the user happened to say this week, the adversarial candidate isn't doing its job. Force a real counter-case.
- **Auto-applying.** Never. The entire point of the weekly cadence is the explicit approval gate.
- **Patch sprawl.** More than 8 patches means the filter is too loose, not that the week was unusually rich.
