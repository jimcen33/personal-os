---
last-updated: 2026-06-20
---

# Validation Prompts

Read this before delivering a **financial conclusion, strategy recommendation, technical architecture proposal, or counterparty-facing draft**. These prompts implement two root-`CLAUDE.md` rules: "verify every number" and "confirm who-is-who / what's the actual flow."

## 1. The validation table (numbers)

Before stating any stacked-percentage, breakeven, total-cost, or date conclusion, build this table and re-check it — then state the answer.

| Component | Value | Sign (+/−) | Source / assumption |
|---|---|---|---|
| e.g. base rate | 2.0% | + | card T&Cs |
| e.g. FX fee | 1.0% | − | issuer schedule |
| e.g. processing fee | 0.3% | − | merchant quote |
| **Net** | **0.7%** | | sum of above |

Rules:
- List **every** component that bears on the result. A dropped fee flips the sign.
- Show the arithmetic, not just the answer.
- Distinguish a *stated conclusion* (must be exact) from a *directional range* ("roughly 3–5 years" is fine if labeled as such).

## 2. Who-is-who / money-flow check (relationships)

Before recommending a financial, tax, or outreach action where a counterparty is named, answer these first — one sentence each:

1. **Who pays whom?** Trace the actual cash/payout direction. Don't assume the textbook model.
2. **Who is the merchant of record / counterparty's real role?** Verify, don't infer from the name.
3. **Which side of the relationship is the named party on?** (e.g. are they the platform, the brand, the agency, the end customer?)

If any answer is a guess, ask one clarifying question before writing the 500-word answer. One sentence of clarification beats a long answer built on the wrong model.

## 3. Lead with the differentiated angle (outreach)

For partnership / BD outreach, lead with the **most specific, relevant claim for that counterparty**, not a generic platform overview. The reader should see in the first line why you're uniquely relevant to *their* world. (Investor/press contexts where a full overview is appropriate are exempt.)
