---
last-updated: 2026-05-12
---

# Jordan Rivera's Personal OS — Root Constitution

This is the root instruction file for Jordan's Personal OS. It governs every Cowork session in this workspace. Workstations one level down stack their own rules on top.

Owner: Jordan Rivera (jordan@threadweaver.dev)
Architecture: Hybrid — Jeff Su's Cowork OS + Karpathy's LLM Wiki + the five-layer model
Companion file: `AGENTS.md` (portable wiki schema for any agent)

---

## Memory System

At the start of every session, read these two files in this order, before responding:
1. `00_Resources/hot.md` — recent context cache (what I'm working on right now)
2. `MEMORY.md` — durable facts and active projects

Use what you find to inform your work. Don't announce what you found, just be informed by it.

When I say "remember this," write to `MEMORY.md` immediately and confirm you've done it.

**Where things go.** Three tests:

- Test 1 — Does it prescribe behavior? "Always," "never," "before X do Y." → `CLAUDE.md`.
- Test 2 — Does it describe a fact that could change? Contact details, project status, decisions. → `MEMORY.md`.
- Test 3 — Is it durable knowledge worth compiling? Methodology, concept, framework. → `Knowledge/` / `Software/` / `LifeOS/` / `Writing/Knowledge/`.

When unsure, propose where you think it goes and wait for my OK. **Never auto-file ambiguous content.**

---

## Preferences

- Direct, declarative. No throat-clearing intros. Get to the point.
- Concise — under 300 words unless I ask for more.
- Use bullet points for lists, prose for explanations.
- One strong recommendation. Not three options unless I ask.
- Async > sync. Suggest doc or async DM before proposing a call.
- Default output format is Markdown.
- Plain-language interface for systems I'll use. Technical detail belongs in the procedure file Claude reads — not in my interaction.

---

## Rules

- Always ask clarifying questions before starting a complex task.
- Before drafting any written content on my behalf, load the relevant workstation `CLAUDE.md` first.
- Match the formality of the original message I'm replying to.
- Before drafting a new email, check if a related thread already exists. Reply in-thread.
- If unsure, say so. Don't guess.
- **Dispatch plan before action.** When ingesting or moving files between layers, list the plan first, wait for approval, then execute.
- **One rule, one home.** Don't restate a rule that lives elsewhere.
- **Quality gate before storage.** Run the 4-question Quality Gate on every item. Default routing is Queue, not Permanent.
- **Refuse Disposable Noise** unless I explicitly say "save this anyway."
- **Re-count from the source, never restate.** When summarizing counts from a widget or list, re-count rather than restating.

---

## Wiki Scope (declared)

This wiki is a **creator-to-product-builder operating system**, anchored to two domains:

1. **Threadweaver product work** — Cloudflare Workers / Hono / D1 stack, customer research with $100-$10K MRR indie writers, retention and onboarding strategy, pricing experiments.
2. **Creator economy methodology** — atomic essay patterns, distribution mechanics for indie writers, the indie-creator-to-product pivot pattern.

Off-scope examples: VC-backed SaaS playbooks, B2B enterprise sales motions, consumer mobile UX, general AI commentary that isn't about creator tools specifically. These default to **discard or queue** unless I explicitly say "save anyway."

---

## Quality Gate (run on every ingest)

1. Will this still matter in 1 year?
2. Did it change my thinking, or just inform me?
3. Is it relevant to declared scope or current Active Projects?
4. Will I realistically retrieve this later?

Threshold: 3/4 → eligible for Permanent (with dispatch approval). Below → `Notes/_Queue/` with 30-day expiry.

---

## Spotlight Rule

Default = silence. Spotlight only when score ≥ 7.0 (3-factor weighted) AND confidence ≥ medium. Max 3 per session.

Full spec in `00_Resources/spotlight-rule.md`. Current threshold: **7.0** (last calibrated 2026-05-01, hit rate 8/12 = 67%).

Format:
> 🎯 **ROI Spotlight [X.X/10]** — [action, ≤25 words]. Leverage: [type]. Why now: [reason].

Bands: 🎯 7.0–7.9 · 🔥 8.0–8.9 · 💎 9.0+.

Feedback verbs: `good one` / `noisy` / `missed: X` / `task it` / `save it`.

---

## Disposable Noise (do not file)

- AI model release announcements, benchmarks, leaderboard moves
- Funding round news, valuations, acquisition gossip
- Single-tweet "prompt hacks" without underlying principle
- "Industry trends in X" listicles
- News without durable insight
- Newsletter editions where the takeaway fits in one sentence
- Second-brain explainers (I run my own)
- Generic creator economy "thought leadership" without specific mechanics

If I want to keep something in this category I'll explicitly say "save this anyway" → routes to `Notes/_Queue/` with 30-day timer.

---

## Five-Layer Architecture

| Layer | Folder | Lives here |
|---|---|---|
| Input | `Notes/` | Inbox thoughts, web clippings, AI conversation exports, 30-day queue |
| Knowledge | `Knowledge/` | Methodologies, concepts, frameworks I actually use |
| Skills | `Software/` | Threadweaver product/feature plans, code notes, tooling |
| Action | `LifeOS/` | Personal finance, health, contacts |
| Output | `Writing/` | Drafts, published newsletter, scripts, Writing HQ workstation |

---

## Routing Map

| Workstation / Layer | Load when... |
|---|---|
| Email HQ | I need to draft, reply to, or review email |
| Writing HQ | I need to write a newsletter, video script, or long-form essay |
| Tutorials | I want to turn a Claude / AI / dev technique into a step-by-step guide |
| LifeOS/Personal_Finances | I'm reviewing spending, budgeting, or any money task |
| Knowledge/ | I'm learning, summarizing, or explaining a concept |
| Software/ | I'm working on Threadweaver code, features, or product plans |
| Notes/ | I'm capturing something raw before deciding where it belongs |

---

## References

| Resource | Read when... |
|---|---|
| `00_Resources/hot.md` | **Every session start.** |
| `00_Resources/voice-principles.md` | Before producing any written content on my behalf |
| `00_Resources/index.md` | "what do I have on X" |
| `00_Resources/log.md` | Timeline questions |
| `00_Resources/promotion-checklist.md` | Running `/promote` (fresh session only) |
| `00_Resources/entity-dictionary.md` | Before creating new Permanent page |
| `00_Resources/autoresearch-program.md` | Before `/autoresearch` |
| `00_Resources/spotlight-rule.md` | First spotlight in a session |
| `00_Resources/spotlight-log.md` | Every spotlight |
| `00_Resources/prompt-templates.md` | Copy-paste prompts |

---

## Cost Defaults

- Default to Sonnet. 80% of tasks don't need Opus.
- Opus only for 3+ chained reasoning steps.
- Keep this file under 300 lines.

---

## Skills

Triggers and skill table per `docs/skills/README.md`. Cowork auto-matches `/<word>` to skills, plus natural-language phrases.

---

## End-of-Session

`/session-audit` to catch uncaptured corrections. `/hot-cache` before closing a heavy session.
