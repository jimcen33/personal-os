# Hot Cache

**Refreshed:** _(empty — run `/hot-cache` at end of your first heavy session)_

This file is the session-to-session continuity layer. ~250 words. Tells the next session what you were working on, what's loaded in your head, and what's about to drop off the radar.

## Format (read first time only)

```
## Refreshed: YYYY-MM-DD

### What I'm working on right now
- [3–5 bullets, current threads]

### Recent decisions (last 2 weeks)
- [bullets]

### Up next
- [bullets]

### Stale / about to fall off
- [bullets — what hasn't been touched in 14+ days]
```

When you trigger `/hot-cache` (or say "wrap up" / "end session"), the agent will rewrite this file based on what happened in the session.
