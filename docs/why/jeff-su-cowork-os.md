# What we borrowed from Jeff Su's Personal OS / Cowork OS

Jeff Su (jeffsu.org) popularized the "Personal OS" framing on YouTube and through his Notion-based templates: a single operating system for your work and life that handles capture, processing, and output across multiple contexts.

His pattern is essentially this template's *workflow* layer, sitting on top of Karpathy's wiki schema.

## What Jeff proposed (paraphrased)

The pattern, as I understand it:

1. **One system for everything.** Don't have separate "knowledge management" and "task management" and "email" tools. One system that routes work to the right sub-context.

2. **Workstations.** Sub-systems within the OS that have their own rules, voice, and workflows. Email is one workstation. Writing is another. Personal Finance is another.

3. **Hot cache.** Recent context that loads on every session start, so the system doesn't ask "what were we working on?" every time.

4. **Five-layer pipeline.** Input -> Knowledge -> Skills -> Action -> Output. Each layer has a different retention rule and a different audience.

5. **Plain-language interface.** The user types human, not config. "Process my notes" works; SQL doesn't.

## What this template takes verbatim

- **Workstations.** Folders with their own `CLAUDE.md` that layer on top of the root constitution. We ship Writing HQ, Email HQ, and Tutorials.
- **Hot cache.** `00_Resources/hot.md` refreshed by `/hot-cache` at session end.
- **Five-layer pipeline.** `Notes/` -> `Knowledge/` -> `Software/` -> `LifeOS/` -> `Writing/`.
- **Plain-language interface.** Skills are addressed by slash commands and natural-language phrases, not config files.

## What we extended

- **The workstation pattern is formalized** with three required files (`CLAUDE.md`, `MEMORY.md`, `<Workstation> Resources/`). Jeff's pattern is more freeform.
- **Memory tier is split** into root `MEMORY.md` (durable facts) + workstation `MEMORY.md` (domain-specific) + `hot.md` (recent context). Cleaner separation.
- **Layered CLAUDE.md.** Workstation rules *layer on top* of root rules, with explicit override semantics. This is a software-engineering pattern (composition over replacement) applied to instruction files.

## What we did differently

- **Quality Gate at ingest.** Jeff's pattern is more capture-friendly. We're stricter at the door - the Quality Gate refuses material before it gets in.
- **Karpathy-style schema.** Jeff's templates are Notion-friendly (database properties, views, relations). We use plain Markdown + frontmatter. Different trade-off: less query power, more portability.
- **Spotlight Rule and Disposable Noise list.** These are new additions not in Jeff's pattern. They reflect a more agentic stance: the system actively gatekeeps, not just routes.

## Where to find the source thinking

Jeff Su's content (jeffsu.org, YouTube, Twitter):
- Personal OS Notion template
- "How I use Notion" video series
- "Cowork OS" framing (more recent)

His templates are Notion-first; this template is Markdown-first. Different stack, same architectural intuitions.

## Why combine them

Karpathy's wiki gives you the *schema* - what knowledge looks like inside an agent-friendly system. Jeff's OS gives you the *workflow* - how knowledge moves through the system as you work.

Wiki without workflow stalls at "I have a great structure but never use it." Workflow without wiki stalls at "I capture everything and find nothing." Combined, they're the substrate this template formalizes.
