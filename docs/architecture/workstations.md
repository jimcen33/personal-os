# Workstations

Sub-systems with their own rules, voice, and workflows. Each workstation is a folder with a `CLAUDE.md` that **layers on top of** the root constitution.

## Why workstations exist

Different output types need different rules. Drafting a long-form essay (Writing HQ) follows a different workflow than triaging an inbox (Email HQ) or turning a technique into a guide (Tutorials). Putting all of these into the root `CLAUDE.md` creates a 1000-line constitution that nobody reads.

Workstations let you keep the root constitution short and load domain-specific rules only when you're actually in that domain.

## The pattern

Each workstation has three things:

1. **`CLAUDE.md`** - workstation rules with these sections:
   - Identity (who this workstation is, what routes here)
   - Resources (table of "Resource | Read when...")
   - Workflow (numbered steps, STOP gates marked)
   - Editorial Rules (opens with: "Follow voice principles in `00_Resources/voice-principles.md`")

2. **`MEMORY.md`** - workstation-specific memory:
   - Contacts (people in this domain)
   - Key Decisions (reasoning behind workstation-specific choices)
   - Recent Activity (running log)

3. **`<Workstation> Resources/`** folder - templates, briefs, persona notes, anything domain-specific

## When to add a workstation

Add one when:
- A task type comes up repeatedly (>= weekly)
- It needs a workflow with multiple steps that aren't obvious to the agent
- It has voice / format rules different from your defaults

Don't add one when:
- The task is ad-hoc (no repeat structure)
- The "rules" are just preferences already covered by root `CLAUDE.md`

## Workstations shipped in this template

- **Writing HQ** (`Writing/Writing HQ/`) - 9-step long-form drafting with STOP gates.
- **Email HQ** (`Email HQ/`) - inbox triage and thread-aware reply drafting.
- **Tutorials** (`Tutorials/`) - turning techniques into non-engineer-friendly guides.

## When to merge workstations

Merge when:
- Two workstations end up with substantially overlapping rules
- One workstation only runs 1-2x a year (move its content into the most-similar active workstation)
- The voice rules between them are converging

## How to add your own

See [docs/customization/adding-a-workstation.md](../customization/adding-a-workstation.md) for the full procedure.
