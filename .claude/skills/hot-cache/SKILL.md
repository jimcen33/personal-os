---
name: hot-cache
description: "Updates 00_Resources/hot.md at end of session so the next session picks up where this one left off. Triggers: /hot-cache, wrap up, end session, cache this, or before closing a heavy working session."
last-updated: "2026-05-17"
---

# Hot Cache (Session-End Bridge)

Refresh `00_Resources/hot.md` so the next Cowork session has full recent context. This is the single biggest UX upgrade in the system - fresh sessions stop being expensive.

## When to use

User says `/hot-cache`, "wrap up", "end session", "cache this", or is about to close a substantial working session.

## Step 1 - Scan the conversation

Look back through this session for:

- **Active work.** What was the user actually trying to do? What's done, what's in flight, what's blocked?
- **Decisions made.** Any "let's do it this way" moments. These belong in `MEMORY.md` Key Decisions, but also surface them here.
- **Open questions.** Things the user asked but didn't resolve. Things flagged but not fixed.
- **File changes.** Which wiki pages, drafts, or workstation files got touched?
- **Workstations used.** Email HQ, Writing HQ, etc.

## Step 2 - Scan recent activity (optional)

If git is available in the workspace, list files modified in the last 7 days. Otherwise, check the last 10 entries in `00_Resources/log.md`.

## Step 3 - Rewrite hot.md

Use this exact structure. Keep total length under 250 words - this file loads on every session start.

```markdown
# Hot Cache - Recent Context

## Refreshed: YYYY-MM-DD

### What I'm working on right now
- <3-5 bullets, current threads>

### Recent decisions (last 2 weeks)
- <bullets>

### Up next
- <bullets>

### Stale / about to fall off
- <bullets - what hasn't been touched in 14+ days>
```

## Step 4 - Update MEMORY.md if needed

If the session produced durable facts that aren't in `MEMORY.md` yet (a new decision worth keeping, a new contact, a project status change), propose the additions to the user.

**Don't auto-write to MEMORY.md.** Propose; let the user confirm.

## Step 5 - Suggest a session summary line

For `00_Resources/log.md`:

```
YYYY-MM-DD | session | <one-line summary of what happened>
```

Append on user approval.

## Failure modes

- **Hallucinating "what I'm working on".** Only list things that actually came up in the session. If the session was short and the user is in a different mode than usual, the hot cache should reflect *this session*, not a generic snapshot.
- **Over-writing useful content.** If hot.md already has high-quality context that wasn't touched this session, keep it - merge new findings in, don't replace.
- **Auto-modifying MEMORY.md.** Never. Always propose first.
