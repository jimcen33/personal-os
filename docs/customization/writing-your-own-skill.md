# Writing Your Own Skill

How to add a new skill (slash command) to the system.

## Skill anatomy

A skill lives at `.claude/skills/<name>/SKILL.md`. The file has frontmatter and a body.

### Frontmatter

```yaml
---
name: <slug>                   # lowercase, hyphen-separated
description: <when to use, <=1024 chars>
last-updated: YYYY-MM-DD
version: <optional - bump on breaking changes>
---
```

The `description` field is **the most important part**. Cowork matches natural-language requests against descriptions, so this field needs:

- A one-sentence opener summarizing what the skill does
- Specific trigger phrases the user might say
- A note on when NOT to use it (if there's a similar skill that could be confused)

Hard cap: 1024 characters. Count them before committing:

```bash
awk '/^---$/{f=!f;next} f && /^description:/{sub(/^description: /,""); print | "wc -c"}' .claude/skills/<name>/SKILL.md
```

### Body

The body should have these sections:

#### When to use
Concrete trigger conditions. What does the user say or do that should invoke this skill?

#### Hard rules
Invariants the skill must never violate. Use these to encode things like "wait for user approval before writing" or "don't promote in-session."

#### Steps
Numbered. Each step:
- Starts with a verb
- Has a clear "you should see X" success criterion
- Marks any STOP gates where user input is required

#### Outputs
What does the skill produce? Files written? A report? A widget?

#### Failure modes
Top 3-5 ways this skill goes wrong, and how to recover.

## Design principles

### 1. One skill, one operation
If a skill does two unrelated things, split it. Skills are easier to debug, test, and improve when they're focused.

### 2. Read the rules first
Almost every skill should start with "Read these resource files first: ..." - it sets context the agent needs.

### 3. STOP gates are real
When you mark a step "STOP - wait for user approval", the agent must wait. No "I'll proceed since this seems obvious." That breaks user trust.

### 4. Reports, not actions, where possible
Skills like `/lint` and `/sync-tasks` report and propose. They don't auto-fix. The user decides.

### 5. Atomic destructive actions
If a skill deletes or moves files, do it in one step. No half-moves. Confirm completion explicitly.

### 6. Cite AGENTS.md
For operations defined in `AGENTS.md` (ingest/query/lint), reference it - don't re-document the schema.

## Example: minimal skill

```markdown
---
name: weekly-review
description: "Generate a weekly review from the last 7 days of work. Pulls from MEMORY.md Recent Activity, hot.md, and the log. Outputs: wins, blockers, open questions, priorities for next week. Triggers: /weekly-review, weekly review, friday review."
last-updated: 2026-05-17
---

# Weekly Review

## When to use
Friday afternoon or whenever the user says /weekly-review.

## Hard rules
- Don't include items >7 days old.
- Don't make up wins. If the week was blocked, say so.

## Steps
1. Read MEMORY.md Recent Activity for the last 7 days.
2. Read 00_Resources/hot.md and 00_Resources/log.md (last 7 days only).
3. Render four sections: Wins, Blockers, Open Questions, Next Week's Priorities.
4. Ask the user to add anything missing.
5. On user approval, save to Notes/Conversation/weekly-review-YYYY-MM-DD.md.

## Outputs
- The review on-screen
- A file saved to Notes/Conversation/

## Failure modes
- Empty week (nothing in MEMORY/hot/log). Say so. Don't fabricate.
- User wants more granularity. Suggest /retrospective for a longer-form version.
```

## Testing your skill

After writing, test it in 3 contexts:

1. **Direct invocation:** type `/<name>` - does it load and run?
2. **Natural-language match:** type a phrase from your description's trigger list - does Cowork route to this skill?
3. **Failure case:** trigger the skill in a state where its preconditions aren't met - does it fail gracefully?

If any of the three is broken, the description or steps need work.

## Sharing your skill

If your skill is generic enough that others would benefit:

1. Open a PR per `CONTRIBUTING.md`.
2. Include `docs/skills/<name>.md` with a deeper explanation if needed.
3. Add a row to `docs/skills/README.md`.
4. Add to root `CLAUDE.md`'s skill table.
