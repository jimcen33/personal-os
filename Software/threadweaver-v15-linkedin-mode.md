---
title: Threadweaver v1.5 — LinkedIn-post output mode
type: methodology
last-updated: 2026-05-15
last-retrieved: 2026-05-14
reliability: high
tags: [threadweaver, product, v1.5, linkedin]
---

# Threadweaver v1.5 — LinkedIn-post output mode

## Goal

Add LinkedIn-post output to Threadweaver. v1 ships X threads only; v1.5 adds LinkedIn. Target ship 2026-06-01.

## Why

- **Customer signal:** 23 of 140 customers have asked for LinkedIn output in the last 60 days (Plain tickets).
- **Distribution shift:** indie writers are migrating engagement back to LinkedIn (anecdotally; some Mixpanel signal that users who use the LinkedIn export-to-PDF feature have 2.4× retention).
- **Pricing leverage:** LinkedIn-mode is the natural upgrade trigger for free → paid.

## Stack notes

- Same Hono router; new endpoint `/api/v1/linkedin/generate`
- New D1 table `linkedin_outputs` (mirrors `thread_outputs` schema)
- Reuses the LLM call layer; different prompt template
- Front-end: new tab in the dashboard, same component skeleton as Threads tab
- Stripe: no new SKU, but bumps power-tier output limit from 50 → 80 per month

## Risks

- **Prompt quality.** LinkedIn-post style is different from X thread style (long-form post, no thread structure, hashtag conventions differ). Prompt iteration risk.
- **Editor UX.** LinkedIn posts are single blocks; X threads are sequences. The editor needs to flex. Tom is designing.
- **Customer expectations.** Users who asked for "LinkedIn" might have meant LinkedIn-article (long-form) not LinkedIn-post (medium). I'll ship LinkedIn-post first; track signal on whether long-form is the gap.

## Timeline

| Date | Milestone | Owner |
|---|---|---|
| 2026-05-08 | API contract finalized | Jordan |
| 2026-05-15 | Backend endpoint shipped to staging | Jordan |
| 2026-05-22 | Frontend UI on staging | Maya |
| 2026-05-26 | Onboarding flow + copy | Tom + Jordan |
| 2026-05-29 | Internal QA + dogfood | All |
| 2026-06-01 | Ship to all customers | Jordan |

## Open questions

- Should the LinkedIn mode require an Open Graph card upload, or auto-generate?
- Should hashtag suggestions be on by default or opt-in?
- Do we charge for LinkedIn output the same as threads, or differently?

## Decision log for this feature

- 2026-04-22 — Decided to ship LinkedIn-post before LinkedIn-article. Reasoning: post format is closer to thread format technically; article format would need a new editor.
- 2026-05-02 — Decided not to bundle Bluesky in this release. Customer signal is weaker; ship LinkedIn first, evaluate Bluesky for v1.6.

## Cross-references

- (TBD) [[linkedin-vs-x-content-norms]]
- [[atomic-essay-pattern]] — the input format users start with
