---
title: "Setting up a customer interview template in Plain"
slug: customer-interview-template-plain
tier: High
priority: 7.4
impact: 8
novelty: 7
effort: 7
last-updated: 2026-05-09
last-verified: 2026-05-09
status: published
related: [creator-to-indie-product-pivot]
---

# Setting up a customer interview template in Plain

## 🧭 Verdict & Comparison

**Priority:** 7.4/10 · **Tier:** High
**Impact:** 8 · **Novelty:** 7 · **Effort:** 7

### Current setup vs. this tutorial

| What I do now | What this tutorial proposes | Why it's different |
|---|---|---|
| Schedule interviews via DM, take notes in a Notion doc, dig through Notion later to find patterns | Use Plain's macros + tags to template the interview flow and tag responses by theme | Searchable, taggable, lives where customer history already lives |
| ~30 min admin overhead per interview (scheduling, taking notes, filing) | ~8 min admin overhead — template loads the structure, tags get applied as I type | Saves ~22 min × 2 interviews/week = 44 min/week |

### Recommendation

**Adopt as-is.** I already use Plain for support; adding the customer-interview workflow there means one less context-switch. Set up takes 30 minutes once.

### Personal / business impact

Faster interview admin = more weekly customer interviews. Currently doing 2/week; could comfortably do 3/week without adding time. Compounds because each interview surfaces signal for [[creator-to-indie-product-pivot]] essays AND product roadmap. The retrieval improvement (tagged transcripts) is the bigger long-term win.

---

## What this tutorial does

By the end, you'll have a Plain macro that loads a structured customer-interview note template, plus a tag schema you apply as you take notes — so interviews are searchable by theme.

## Who this is for

- Solo founder doing customer interviews
- Already using Plain for support (or willing to)
- No prior knowledge of Plain macros
- Estimated time: 30 minutes

## Before you start

- Plain account with admin access to settings
- 3-5 sample interview notes you've already taken somewhere else (Notion, doc, etc.)

## Steps

### Step 1: Identify your themes

Look at your last 3-5 interview notes. What categories of insight come up? For me:
- `pain-point/onboarding`
- `pain-point/feature-gap`
- `praise/voice-fit`
- `praise/output-quality`
- `pricing/sensitivity`
- `churn/why`

Write down 5-10 themes. Don't over-think — you can add more later.

> **You should see:** a small list of 5-10 themes in your notes.

### Step 2: Create tags in Plain

In Plain: Settings → Tags → New Tag.

For each theme above:
- Tag name: the slash-style name (e.g. `pain-point/onboarding`)
- Color: pick a color per category prefix (red for pain-points, green for praise, etc.)

> **You should see:** 5-10 new tags in your tag list.

> **If you don't see the Tags option in Settings:** you may need to be the org admin in Plain. Check Settings → Team.

### Step 3: Create the interview macro

In Plain: Settings → Macros → New Macro.

Name it: "Customer interview template"

Body:
```
## Customer interview — {date}

**Customer:** {customer name}
**Stage:** [trial | new (<30d) | power (>3mo) | churned]

### Q1. What were you trying to do when you signed up?
-

### Q2. What did you try before this?
-

### Q3. Walk me through your first 5 minutes after signup.
-

### Q4. What's missing or broken?
-

### Q5. What's working? What would you tell a friend?
-

### Q6. (Churned) What would have kept you?
-

---

### My notes / themes
-

### Action items
- [ ]

### Tags to apply
(Apply tags from the tag list as you take notes — pain-point/*, praise/*, pricing/*, churn/*)
```

Save the macro.

> **You should see:** "Customer interview template" in your macro list, available via `/macros` in any thread.

### Step 4: Schedule + run an interview

When you book a customer interview, open a Plain thread with that customer (or create one). Type `/macros` → pick "Customer interview template". The template loads.

During the call, fill in answers. Apply tags as you go — when a customer mentions onboarding friction, hit the `pain-point/onboarding` tag right then.

> **You should see:** the thread filled in with the structured template, with tags applied as you went.

### Step 5: Search by theme later

Plain → Search → filter by tag. Pull every `pain-point/onboarding` mention across all customers in the last 90 days.

This is the payoff. Now you can see patterns across customers, not just within one interview.

> **Success looks like:** a search showing 5-10 mentions of the same theme across different customers. That's your roadmap signal.

## Troubleshooting

### Problem: tags don't appear on individual messages
**Fix:** In Plain, tags apply to *threads*, not messages. Apply to the whole thread.

### Problem: I want to export all `pain-point/*` mentions
**Fix:** As of 2026-05-09 Plain doesn't have native cross-tag export. Workaround: search by tag, copy threads manually. Or use the Plain API to script it (out of scope for this tutorial).

## What you can do next

- Set up a monthly "interview signal" review: filter by tag for the last 30 days, write up patterns
- Add a `churn-risk` tag for power users showing reduced engagement
- Connect Plain → newsletter brief seeds (open question for me: not solved yet)

## Reference

- [Plain macros docs](https://www.plain.com/docs/macros)
- [Plain tags docs](https://www.plain.com/docs/tags)
- [[creator-to-indie-product-pivot]] (why customer interview cadence matters)

## Notes for future maintenance

- **UI version:** Plain dashboard, May 2026
- **Last verified working:** 2026-05-09
- **Known UI changes since:** None as of last verify
