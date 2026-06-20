---
last-updated: 2026-06-20
---

# run-log.jsonl — observability spec

Every heavy skill and every scheduled/autonomous task appends **exactly one JSONL line** to `00_Resources/run-log.jsonl` at the end of its run. This is the OS observability layer: `/lint` reads the last ~14 days to flag silent runs, recurring errors, and escalation droughts.

Interactive one-shot skills (`/wiki`, `/new-info`) are **exempt** unless you specifically want latency tracking.

## Line schema

One JSON object per line. No trailing commas, no comments inside the file (JSONL is strict).

```json
{"ts":"2026-06-20T14:03:00Z","skill":"ingest","trigger":"manual","duration_s":92,"items_in":7,"items_out":5,"escalations":0,"errors":0,"notes":"2 routed to queue, 3 permanent"}
```

| Field | Type | Meaning |
|---|---|---|
| `ts` | string (ISO 8601 UTC) | When the run finished. |
| `skill` | string | Skill or task name, e.g. `ingest`, `autoresearch`, `weekly-taste-sync`. |
| `trigger` | string | `manual`, `scheduled`, or the upstream skill that called it. |
| `duration_s` | number | Wall-clock seconds. |
| `items_in` | number | Items the run started with (files scanned, queue items, etc.). |
| `items_out` | number | Items produced/changed. |
| `escalations` | number | Loop-guardrail breaches or HALTs (≥1 means something stopped early). |
| `errors` | number | Hard errors encountered. |
| `notes` | string | One short human-readable line. On a guardrail breach, put the stop reason here. |

## How `/lint` uses it

- **Silent run** — a scheduled skill that should have logged in the window but didn't.
- **Recurring errors** — same `skill` with `errors >= 1` across multiple runs.
- **Escalation drought** — a loop skill that *never* escalates is suspicious (its guardrails may be dead code).

## Loop guardrails (see root `CLAUDE.md`)

Any skill that iterates unattended logs an escalation line (`escalations >= 1`, reason in `notes`) instead of continuing when it breaches any of: iteration cap, no-progress detection, or budget ceiling. Default caps where unspecified: **3 rounds · 10 min · halt-on-zero-progress**.
