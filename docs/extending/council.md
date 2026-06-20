# Extending: the Council (role-specialized decision agents)

The Council is an **optional** layer for breaking confirmation bias on high-leverage decisions. It pairs role-specialized *lenses* with a chartered Skeptic. If you don't want it, delete `.claude/council/` and `.claude/skills/skeptic/` — nothing else depends on it.

## The idea

A single agent tends to agree with the framing of whoever is asking. The Council fixes this two ways:

1. **Role lenses.** Each role is a context file (`.claude/council/<role>/CONTEXT.md`) that views every decision through one specialized perspective — and is explicitly chartered to disagree with the other roles and with you.
2. **The Skeptic.** A structural dissent voice (`/skeptic`) that runs *after* the roles weigh in and *before* you decide. It returns 3 ranked failure modes, 1 contrarian alternative, 1 kill criterion, and 1 concession.

## Setup

1. Decide your 3–5 roles. Pick the lenses that matter for *your* recurring decisions. Generic starting set:
   - **Revenue / Sales** — economics, pricing, go-to-market.
   - **Product** — scope, build-vs-buy, what to cut.
   - **Brand / Content** — narrative coherence, audience fit.
   - **Competitor / Market** — competitive read, external risk.
   - **Finance / Ops** — runway, breakeven arithmetic, capital.
2. Copy `.claude/council/_template/CONTEXT.md` into `.claude/council/<role>/CONTEXT.md` for each role and fill it in.
3. Keep each role file a **derivative** of your source-of-truth docs (your wiki, finances, product plans). The wiki is canonical; the council files are lenses on it. When the underlying facts change, refresh the matching `CONTEXT.md`.

## How to invoke

For a major decision:

1. Load the relevant role's `CONTEXT.md` into the session.
2. Let that role weigh in *in its own voice* (it should push back, not agree).
3. Run `/skeptic "<the proposal>"`.
4. Decide.

## Calibration (the part that keeps it honest)

Every `/skeptic` run logs to `00_Resources/council-log.md`. Audit it every 10 invocations:

- **Target Skeptic accuracy: 40–50%** ("the concern was valid in hindsight"). Much higher than 50% means it's a yes-man dressed as a critic. Much lower means it's noise.
- If the same role agrees with you on every decision, its `CONTEXT.md` charter is too soft — rewrite it to bias toward dissent.

## Why not just one agent with a "be critical" instruction?

Because a single critical instruction degrades over a long session — the agent drifts back to agreeable. Separate, chartered role files and a logged accuracy loop make the dissent *structural* instead of *vibes-based*.
