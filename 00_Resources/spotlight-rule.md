---
last-updated: 2026-05-17
---

# Spotlight Rule — Full Spec

The summary is in root `CLAUDE.md`. This file holds the full procedure, the rubric definitions, the anti-fatigue logic, and the calibration SOP. Read it the first time the rule fires in a session.

---

## Why this rule exists

LLMs default to surfacing lots of "you might find this interesting" — flattering, useless, training the user to ignore the assistant. The Spotlight Rule prevents that by:

1. **Setting a quantitative bar.** Score ≥ 7.0 on a weighted 3-factor rubric.
2. **Forcing first-principles checking.** The Surprise dimension scores against the user's existing model — sycophantic spotlights can't pass.
3. **Capping volume.** Max 3 per session.
4. **Auto-calibrating.** Every spotlight is logged with an outcome field; the threshold adjusts based on hit rate.

---

## The rubric

Each factor scored 1–10, then weighted sum:

### Leverage Type (× 0.4)
Naval Ravikant's leverage stack: which type of leverage does this insight unlock?

| Score | Leverage Type |
|---|---|
| 10 | Permissionless code/media building durable moat (a software primitive, an evergreen piece of content) |
| 7 | Capital deployed against an asymmetric bet |
| 5 | Labor leverage (hiring, delegation) |
| 3 | Process leverage on your own work |
| 1 | Pure labor — you doing the work yourself |

### Specific Knowledge (× 0.3)
Does this insight sharpen *your defensible edge*, or is it generic?

| Score | Specific Knowledge |
|---|---|
| 10 | Sharpens your unique edge — something no one else with your exact stack knows |
| 7 | Domain-specific, applies to your industry |
| 5 | Generic ops knowledge (good time management, productivity) |
| 3 | Commodity info available in any business book |
| 1 | Truisms |

### First-Principles Surprise (× 0.3)
Does this challenge or confirm your existing model?

| Score | Surprise |
|---|---|
| 10 | Breaks an assumption (the cost is 10× lower / the impact 10× higher than you assumed) |
| 7 | Materially shifts your model |
| 5 | Confirms your current model with new evidence |
| 3 | Restates something you already knew |
| 1 | Truism / known-known |

**This dimension is the anti-sycophancy mechanism.** A spotlight that just agrees with the user can't reach 7.0 because Surprise drags it down.

---

## Computing the score

```
score = (leverage × 0.4) + (specific_knowledge × 0.3) + (surprise × 0.3)
```

**Spotlight fires only when `score ≥ 7.0` AND source confidence is `medium` or `high`.**

### Emoji band
- 🎯 7.0–7.9 — queue it
- 🔥 8.0–8.9 — do this week
- 💎 9.0–10.0 — drop other things

---

## Output format

```
🎯 ROI Spotlight [X.X/10] — <action or insight, ≤25 words>. Leverage: <type>. Why now: <one-clause reason>.
```

Example:
> 🔥 **ROI Spotlight [8.2/10]** — The customer interview pattern from Page A actually breaks your retention assumption from Page B — re-test the onboarding hypothesis this week. Leverage: specific knowledge. Why now: you're in the middle of redesigning onboarding.

---

## Anti-fatigue rules

### Per-session ceiling
Max 3 spotlights per session. If a 4th candidate appears, it must beat the lowest already-emitted by ≥0.5 to replace it. (E.g., a 7.2 spotlight is already emitted; a new 7.6 candidate replaces it; a new 7.4 candidate does not.)

### Threshold adjustment
End of every session:
- If user acknowledged at least one spotlight (with a feedback verb like `good one` or `task it`) → keep threshold at 7.0 for next session
- If user did not acknowledge any → raise threshold to 7.5 for next session
- After 3 consecutive un-acknowledged sessions → raise to 8.0 and consider whether the rule should be temporarily muted

### Feedback verbs

Verbs the user can say after any spotlight:

| Verb | Effect |
|---|---|
| `good one` | Mark this spotlight as `outcome: hit` in spotlight-log |
| `noisy` | Mark `outcome: miss`. If 3 consecutive `noisy`, raise threshold |
| `missed: <thing>` | Log a candidate the agent failed to spotlight |
| `too many spotlights` | Cap session at 1 |
| `I'm missing things` | Lower threshold to 6.5 for this session |

Action routing verbs (can stack with reaction verbs):

| Verb | Effect |
|---|---|
| `task it` | Create a task file (depends on your task workflow) |
| `save it` | Drop a 30-day draft in `Notes/_Queue/` |
| `share to <vault>` | Stage in `Notes/Inbox/` for cross-vault routing |

Example: `good one, task it and save it` — marks hit, creates a task, drops in queue.

---

## Calibration loop

Every spotlight is appended to `spotlight-log.md` with a blank `outcome:` field. At 30 log entries (or when the user invokes `/calibrate-spotlight`):

1. Count outcomes: `hit` / `miss` / `unmarked`
2. If hit-rate < 30% → raise threshold by 0.5
3. If hit-rate > 70% → lower threshold by 0.5
4. Document the adjustment in spotlight-log
5. Reset to start the next 30-entry window

This is the loop that prevents the rule from going stale. Without it, the threshold drifts to whatever the agent's prior is, which is usually too low.

---

## When to break the rule

The Spotlight Rule is a *default*, not a law. Override cases:

- **User explicitly asks** for the agent's opinion on something — the rule doesn't apply (they invited noise).
- **High-stakes safety** — a real risk to money/health/legal status should be surfaced even if the score is below threshold.
- **Active project triage** — when the user is in `/sprint-planning` or `/pipeline-review` style mode, the agent is *expected* to surface more candidates.

In all override cases, the agent should still log the override to spotlight-log with a note explaining why.
