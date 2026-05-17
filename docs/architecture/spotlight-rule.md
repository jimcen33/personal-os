# The Spotlight Rule

The full operational spec is in `00_Resources/spotlight-rule.md`. This doc explains the *why* - the design choices behind the rule and what problems it solves.

## The problem it solves

LLM assistants default to surfacing lots of "you might find this interesting." This creates three problems:

1. **Volume fatigue.** Within a week, the user starts ignoring all surfaced items.
2. **Sycophancy.** The agent surfaces things the user already agrees with, because those are easier to score as "high value."
3. **Stale signal.** With no calibration loop, the threshold drifts to whatever the agent's prior is - usually too low.

The Spotlight Rule fixes all three.

## The mechanism

### A quantitative bar
Score >= 7.0 on a 3-factor weighted rubric, with confidence >= medium. Below this, default behavior is silence.

### A first-principles forcing function
The Surprise dimension (weighted 0.3) scores against the user's existing model. Specifically:
- 10 = breaks an assumption
- 5 = confirms current model
- 1 = restates known

**A spotlight that just confirms the user's prior can't reach 7.0**, because Surprise drags the score down. This is the anti-sycophancy mechanism.

### A volume cap
Max 3 spotlights per session. A 4th candidate must beat the lowest emitted by >=0.5 to replace it. This prevents the agent from spamming high-scoring items in a single session.

### A calibration loop
Every spotlight is logged with a blank `outcome:` field. The user marks `hit` / `miss` over time. After 30 entries, the threshold adjusts:
- <30% hit-rate -> raise threshold by 0.5
- >70% hit-rate -> lower threshold by 0.5

This is the loop that keeps the rule from going stale.

## The rubric in detail

### Leverage Type (weight 0.4)

Based on Naval Ravikant's leverage stack:

| Score | Type | Example |
|---|---|---|
| 10 | Permissionless code/media | A software primitive, evergreen piece of content |
| 7 | Capital | An asymmetric financial bet |
| 5 | Labor leverage | Hiring, delegation |
| 3 | Process leverage | Better SOPs on your own work |
| 1 | Pure labor | You doing the work yourself |

### Specific Knowledge (weight 0.3)

| Score | Type | Example |
|---|---|---|
| 10 | Defensible edge | Something no one with your exact stack knows |
| 7 | Domain-specific | Applies to your industry |
| 5 | Generic ops | Time management, productivity |
| 3 | Commodity info | Available in any business book |
| 1 | Truism | "Hard work pays off" |

### First-Principles Surprise (weight 0.3)

| Score | Type | Example |
|---|---|---|
| 10 | Breaks an assumption | "Cost is 10x lower / impact 10x higher than assumed" |
| 7 | Material shift | Updates your model meaningfully |
| 5 | Confirms model | New evidence for what you already believed |
| 3 | Restates known | Heard this before |
| 1 | Truism | Known-known |

## Why these weights

- **Leverage 0.4** because leverage is the single biggest determinant of impact at the founder/creator scale.
- **Specific Knowledge 0.3** because edge-sharpening insights compound across your entire career, while generic ops insights have flat returns.
- **Surprise 0.3** to push back hard against sycophancy without dominating the score.

If you weight Surprise higher (say 0.5), the rule starts firing on contrarian-for-its-own-sake material. 0.3 is enough to prevent "you're right" loops without rewarding edginess.

## When to break the rule

The Spotlight Rule is a default, not a law. Override cases:

- **User explicitly asks for the agent's opinion** - they invited input, the rule doesn't apply.
- **High-stakes safety** - real risk to money/health/legal should surface regardless of score.
- **Active triage modes** - during `/sprint-planning` or `/pipeline-review` the user expects multiple candidates.

In overrides, still log to `spotlight-log.md` with an "override" note - keeps the calibration data clean.

## What this rule does NOT do

- **It doesn't replace human judgment.** A spotlight is "consider this," not "do this."
- **It doesn't catch everything.** Some valuable items will score below 7.0 because they're high-impact but high-effort. That's fine - they live in normal task surfacing, just not in spotlight surfacing.
- **It doesn't work on day one.** The calibration loop needs ~30 entries to start tuning. For the first month, use the default 7.0 threshold; expect some misses.
