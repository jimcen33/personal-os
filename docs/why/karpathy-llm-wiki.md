# What we borrowed from Karpathy's LLM Wiki

Andrej Karpathy has talked publicly (on X and in various conversations) about the idea of a personal wiki structured specifically for LLM consumption - not human reading first, but agent reading first. This template takes that core idea and operationalizes it.

## What Karpathy proposed (paraphrased)

The core insights, as I understand them:

1. **Typed pages.** Instead of free-form notes, pages have a declared type (concept, methodology, decision, reference). The type tells the agent how to parse and use the page.

2. **Agent-readable schema.** The wiki has an `AGENTS.md`-style file that documents the schema in a form an agent can consume directly, without reading a human-facing README.

3. **Quality bar over quantity.** The wiki refuses low-value material. A small, high-quality wiki outperforms a large, mediocre one.

4. **Wiki as substrate for an agent.** The wiki isn't just a search target - it's where the agent's knowledge and your knowledge co-exist. The agent reads the wiki, writes back to it, and improves it over time.

5. **Cross-page connections matter.** Wikilinks, contradiction detection, and orphan-finding are first-class operations, not afterthoughts.

## What this template takes verbatim

- **Typed pages.** Frontmatter `type:` field with a small enum (methodology / concept / decision / reference / decomposed / queue).
- **`AGENTS.md`.** Portable wiki schema, designed to be the entry point for any agent.
- **Quality bar.** The 4-question Quality Gate refuses low-value material at ingest, not at retrieval.
- **Cross-page connections.** `/wiki` surfaces missing links; `/lint` finds orphans, broken links, contradictions; `/connections` finds cross-idea patterns.

## What we extended

- **The Quality Gate is operationalized as 4 specific scoring questions.** Karpathy talked about a quality bar; this template makes it concrete and testable.
- **Three-zone ingest format.** Verbatim / metadata / decomposed SOP. This is a packaging on top of the typed-page idea.
- **Zone C decomposition.** The 6-step methodology-to-SOP transform. New.
- **The Disposable Noise refusal list.** Explicit categories the agent refuses to file. New.
- **The Spotlight Rule.** Anti-sycophancy mechanism with a calibration loop. New, mine.

## What we left out

Some directions Karpathy mentioned that this template doesn't pursue:

- **Auto-promotion.** Karpathy talked about agents that improve the wiki autonomously. This template requires human-in-the-loop at every promotion. We think the friction is the point.
- **Heavy structured-data fields.** This template uses lightweight YAML frontmatter, not deep structured schemas. Easier to maintain.
- **Graph-database backends.** This template is plain Markdown. The graph is implicit in `[[wikilinks]]`.

## Where to find the source thinking

Karpathy's public conversations on this topic are scattered across X posts and podcast appearances. Notable references (search rather than direct-linking since posts move):

- "@karpathy LLM wiki" search on X
- Lex Fridman podcast episodes featuring Karpathy
- Karpathy's GitHub: github.com/karpathy

This template is one specific instantiation of his ideas - not a definitive interpretation. Your mileage may vary; his version may differ.
