---
name: lint
description: "Run the 7-check Lint operation on a layer. Checks: Orphans, Stale, Contradictions (with in-page callouts), Broken links, Node Decay, Gaps, Zone C Drift (decomposed pages that drifted back to summary mode). Reports findings; drops contradiction callouts directly into conflicting pages. Triggers: /lint, lint my wiki, health check, wiki audit."
last-updated: "2026-05-17"
version: v2
---

# Lint

Run the Lint operation from `AGENTS.md`. Seven checks per layer. Reports findings - does NOT auto-fix. The one write-action allowed is dropping `[!contradiction]` callouts into conflicting pages, because contradictions are most useful when surfaced in-context.

## When to use

User says `/lint`, "lint my wiki", "health check", "wiki audit", or runs a periodic maintenance cadence (typically monthly).

## Target layer

Ask the user which layer to lint if not specified. Valid: `Knowledge`, `Software`, `LifeOS`, `Writing/Knowledge`, `Notes/_Queue`, or `all`.

If `all`, run each layer in sequence, report each separately. Don't merge findings.

## Seven checks

### 1. Orphans
Pages with zero inbound wikilinks (excluding `index.md`, `log.md`, `entity-dictionary.md`, `hot.md`, `README.md`, `_*.md` templates).

Skip this check for `Software/` (mostly standalone manuals; orphans are the norm).

### 2. Stale
Pages last modified >90 days ago in domains where currency matters (`LifeOS/Investing`, `LifeOS/Health`, `LifeOS/Personal_Finances`, `Writing/Drafts`, `Software/` for tools that update fast).

### 3. Contradictions (in-page + report)
Two pages making opposite claims on the same topic. Use `00_Resources/entity-dictionary.md` to find pairs that should agree on key claims.

**Drop an `[!contradiction]` callout into BOTH pages**:

```markdown
> [!contradiction] Conflicts with [[other-page]]
> This page says X; [[other-page]] says Y. Resolve before relying on either.
```

Don't auto-resolve. The user picks the canonical page and patches the other.

### 4. Broken links
`[[wikilinks]]` pointing to nonexistent pages. Report path + suggested-correct-path if a typo is obvious.

### 5. Node Decay
Pages where `last-retrieved` (set by `/wiki` queries) is >90 days old. The page exists but is never actually used. Candidate for archival or merge.

### 6. Gaps
Entities referenced >=3 times across the wiki without a canonical page in `entity-dictionary.md`. Report as `gap: <entity-name>` candidates for promotion.

### 7. Zone C Drift
`type: decomposed` pages where the Zone C section has drifted into summary mode. Check for the refusal heuristic: does each step of the SOP start with a verb? Is there a "you should see X" criterion? Or has it become "this article discusses..."?

If drifted, report the page path and suggest re-running the Zone C transform.

## Output format

```
## Lint Report - <layer> - <date>

### 1. Orphans (N)
- [[page-a]]
- [[page-b]]

### 2. Stale (N)
- [[page-a]] - last-updated 142 days ago

### 3. Contradictions (N)
- [[page-a]] vs [[page-b]] - on the claim: "..."
  Callouts dropped in both pages.

### 4. Broken links (N)
- [[wrong-name]] in [[page-a]] (line 42) - did you mean [[correct-name]]?

### 5. Node decay (N)
- [[page-a]] - last-retrieved 97 days ago

### 6. Gaps (N)
- gap: <entity> - referenced in [[page-a]], [[page-b]], [[page-c]]

### 7. Zone C drift (N)
- [[page-a]] - SOP section drifted to summary mode; re-decompose

### Summary
<one paragraph: top 3 issues to address>
```

## Failure modes

- **Auto-fixing.** Don't. The agent's mental model of which page is canonical is often wrong. Report. The user fixes.
- **Linting the wrong layer.** Confirm the target layer first.
- **Cross-layer contradictions.** When the conflicting pages are in different layers (e.g., `Knowledge/` and `LifeOS/`), the callout still gets dropped in both. Cross-layer contradictions are usually the most useful kind.
- **Spurious orphans.** Some pages are intentionally orphan (an index, a glossary, a one-off log). Maintain an `exclude` list in the orphan check or just skip those by name.
