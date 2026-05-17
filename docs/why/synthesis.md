# Synthesis - Why Combine Karpathy + Jeff Su

This template is what you get when you fuse Karpathy's LLM-wiki schema with Jeff Su's Personal OS workflow, and add the connective tissue that makes them work as one artifact.

## The mismatch they solve

### Karpathy's wiki alone
Strengths: clean schema, agent-readable, quality-first.
Weakness: doesn't tell you *when* to ingest, when to lint, when to query. The wiki is a destination, not a workflow.

### Jeff Su's Personal OS alone
Strengths: real workflow, workstations, plain-language interface, recent-context cache.
Weakness: Notion-bound, schema is implicit (relations + properties), no operational rigor on what gets in (capture-friendly).

### Combined
The wiki gives you the *what* (schema, typed pages, governance). The OS gives you the *when* (workstations, hot cache, routing). The result is a system where the agent always knows:
1. **What this material is** (type, layer, reliability)
2. **What to do with it** (ingest / queue / refuse / promote)
3. **When you'll need it** (retrieval surface tuned by `last-retrieved`, scope, active projects)

## The connective tissue we added

Karpathy + Jeff Su don't quite meet in the middle. Pieces this template adds:

### 1. The Quality Gate (4 questions)
Karpathy talks about a quality bar; Jeff's pattern is capture-friendly. The 4-question gate is the formal mechanism that says "stop captures here unless they pass." Without it, the wiki bloats.

### 2. Three-zone ingest (Verbatim / Metadata / Decomposed)
Karpathy's typed pages are clean; Jeff's pages are practical. Zone A preserves the verbatim source (Karpathy's "structured data matters"); Zone B adds the metadata (frontmatter, links); Zone C decomposes methodology sources into actionable SOPs (mine - the "what do you DO Monday morning" refusal).

### 3. The Spotlight Rule
Neither source addresses sycophancy directly. The Spotlight Rule is a 3-factor weighted score with a calibration loop. Without it, an LLM-driven wiki devolves into "you might find this interesting" spam.

### 4. The Disposable Noise refusal list
Explicit. Categorical. The agent refuses to file model releases, funding gossip, "10 things you need to know" listicles. Not in Karpathy or Jeff - this came from running the system and noticing what *always* slipped past the gate.

### 5. Workstation-on-top-of-root composition
Jeff has workstations; Karpathy doesn't really. Karpathy has typed pages; Jeff doesn't really. Composing them - workstations with their own typed pages, voice rules, and workflow - is the integration move.

### 6. Multi-vault constellation pattern
Neither source addresses what happens when one wiki splits into multiple by sensitivity profile. This template documents the pattern (and the sandbox constraints) so you can grow into it.

## The philosophy you get

Combine all of it and the system has a stance:

- **Refuse more than you accept.** Most material isn't worth keeping.
- **Decompose, don't summarize.** A methodology becomes an SOP. "What does this article say" is the wrong frame; "what do you do Monday morning" is the right one.
- **The agent is opinionated.** It scores, it refuses, it surfaces what surprises. Sycophancy is structurally prevented.
- **The human is always in the loop on writes.** Auto-fix is a trap. Propose; the user approves; then write.
- **Plain Markdown is sufficient.** No database, no plugins, no lock-in.

If you disagree with any of these, fork. They're all on by default for a reason, but they're all editable.

## What this isn't

- Not a "second brain" tool. Second brains capture; this system *refuses*.
- Not a project management tool. Tasks live in a separate system you reference from `MEMORY.md` Active Projects.
- Not a chatbot wrapper. The agent operates the wiki; the wiki isn't just a context dump for the agent.
- Not a Notion / Obsidian competitor. It's an instruction layer that runs on top of plain files - it doesn't replace your editor.

## When this is the wrong tool

Don't use this template if:

- You want zero structure - this is opinionated, you'll fight it.
- You want capture-first speed at all costs - the Quality Gate slows you down on purpose.
- You don't run an LLM agent against your notes - the system is designed *for* an agent. Without one, you're just maintaining structure manually.
- Your knowledge is heavily structured (a literature review with thousands of papers, a customer database) - those want a real database, not a markdown wiki.

For everyone else - solo founders, creators, researchers, anyone running a personal knowledge system with an agent - this is what we built.
