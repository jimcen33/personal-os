---
name: wiki
description: "Query the wiki for an open question. Cites every claim with wikilinks. Surfaces missing cross-links for approval. Updates last-retrieved frontmatter on every page used. Triggers: /wiki, what do I know about X, find my notes on Y, or any open question that should be answered from the wiki."
last-updated: "2026-05-17"
---

# Wiki Query

Run the Query operation defined in `AGENTS.md`. Search, synthesize, cite, surface missing cross-links.

## When to use

User says `/wiki <question>`, "what do I know about X", "find my notes on Y", or asks any open question that should be answered from the wiki rather than the model's training data.

## Step 1 - Confirm the question

If the user hasn't stated a clear question, ask before searching. A vague query produces a vague answer.

## Step 2 - Identify relevant layers

Use the routing map in root `CLAUDE.md` to pick the layer(s) most likely to contain the answer:

- Methodology / concept question -> `Knowledge/`
- Tooling / code / product question -> `Software/`
- Money / health / legal / contacts -> `LifeOS/`
- Writing-craft question -> `Writing/Knowledge/`
- Recently captured / not yet promoted -> also check `Notes/_Queue/`

## Step 3 - Search

Search file names, frontmatter tags, then full text. Don't read the whole wiki; read the candidates.

Surface 3-7 candidate pages before reading. Then read the most relevant 3-5.

## Step 4 - Answer with citations

Synthesize across multiple pages. **Cite every claim** with a `[[wikilink]]` to the source page.

Format example: `As noted in [[claude-code-seo-masterclass]], indexing prefers explicit canonical tags over rel=canonical headers.`

If a claim has multiple sources, cite all relevant pages.

## Step 5 - Update last-retrieved

For every page actually used in the answer, update its frontmatter `last-retrieved: <today's date>`. This is the anchor for the Node Decay check in `/lint`.

## Step 6 - Flag reliability

If a cited page has `reliability: low` or `reliability: medium`, surface that at the end of the answer:

```
> Reliability note: [[page-x]] is tagged `reliability: medium` (single source, not cross-verified). Treat its claim about Y as provisional.
```

## Step 7 - Surface missing cross-links

If two pages should link to each other but don't, surface the connection. Format:

```
> Suggested cross-link: [[page-a]] and [[page-b]] both discuss <topic>. They don't currently link. Want me to add a link?
```

The user confirms before patching.

## Failure modes

- **Searching outside the wiki.** This skill is for *what's in the wiki*, not web research. If the wiki has nothing, say so - don't fall back to model knowledge silently. Suggest `/autoresearch` if web research is needed.
- **Citing pages you didn't actually read.** If you only saw the title in a search result, don't cite. Read the page or omit.
- **Missing cross-links you should propose.** Be aggressive about surfacing connections. The wiki gets better every time a query is run.
- **Over-citing.** If 4 pages all say the same thing, cite the most reliable one with a note like "(also covered in [[a]], [[b]], [[c]])".
