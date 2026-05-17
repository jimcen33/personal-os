---
last-updated: 2026-05-17
---

# Autoresearch Program

Source preferences and constraints for the `/autoresearch` skill. Read by the skill before it runs a 3-round research pass.

---

## When `/autoresearch` runs

User invokes `/autoresearch <topic>` or says "deep dive on X" or "research Y." The skill performs three rounds of web search + synthesis, fills gaps surfaced by lint, and files results into the wiki (default destination: `Notes/_Queue/`).

---

## Source preferences

### Trust order (highest → lowest)

1. **Primary sources** — the actual paper, doc, repo, announcement.
2. **Author-authored synthesis** — the author of the work writing about their work.
3. **Trusted secondary** — domain-respected publications, well-cited blogs.
4. **General secondary** — well-known news / tech publications.
5. **Aggregators / summaries** — only as a starting point; never citable.

Cowork should cite primary sources whenever possible. If only an aggregator is available, mark the finding as `reliability: low`.

### Domain-specific defaults

> Edit this list to match your domain. Examples below.

- **Academic / research** — prefer arxiv.org, official journal pages, official researcher pages over Medium / Substack summaries.
- **Software engineering** — prefer official docs, RFCs, source code, maintainer blog over Stack Overflow.
- **Business / market data** — prefer the actual S-1 / 10-K / earnings call over a journalist's summary.
- **Health / medical** — prefer peer-reviewed primary studies or official health agency sources (NIH, NHS, etc.); explicitly downrank press-release summaries.

---

## Constraints

- **Refuse aggregator-only findings.** If the only source is a "10 things you need to know about X" listicle, return "insufficient quality sources, retry with narrower query."
- **Cross-verify with 2+ independent sources** before promoting a finding from `reliability: medium` → `reliability: high`.
- **Flag conflict, don't resolve.** If two trusted sources disagree, capture both with their source links — let the user adjudicate.
- **Refuse predictions / forecasts** unless explicitly requested. Stick to what's known.
- **No paywall scraping.** If the primary source is paywalled and the user wants it, ask them to share access.

---

## Output format

### Round 1 — broad scan
- 5–8 sources, mixed quality, tagged with trust level
- One-paragraph synthesis identifying the 3–5 sub-questions worth deeper research

### Round 2 — depth on sub-questions
- For each sub-question: 2–3 high-trust sources
- Identify remaining gaps (questions still unanswered)

### Round 3 — gap-filling
- Targeted search on remaining gaps
- Final synthesis with full citation list

### Final deliverable
- Markdown file in `Notes/_Queue/` with frontmatter
- `gate-score` estimated by the agent (will be re-evaluated at promotion)
- Full citation list at the bottom
- Open questions section if not all gaps filled

---

## Anti-patterns

- Don't do 3 rounds on a topic that can be answered in 1 search. Cap at the depth the topic actually needs.
- Don't synthesize when sources disagree — surface the disagreement, name the sources.
- Don't ingest aggregator content as if it were primary.
