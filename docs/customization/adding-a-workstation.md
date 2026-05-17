# Adding a Workstation

How to extend the system with a new workstation when an existing one doesn't cover what you need.

## When you need a new one

You should add a workstation when **all four** are true:

1. The task comes up at least weekly.
2. It has a multi-step workflow that isn't obvious to the agent.
3. It has voice or format rules different from your defaults.
4. The existing workstations don't already cover it.

If only 1-2 are true, layer the rules into an existing workstation or into root `CLAUDE.md`. Workstations are for cohesive *sub-systems*, not for every preference.

## The procedure

### 1. Create the folder

```
mkdir "Your Workstation Name"
mkdir "Your Workstation Name/Your Workstation Name Resources"
```

Folder names are human-readable, not slugified. The folder name shows up in chat when the agent references the workstation.

### 2. Create `CLAUDE.md`

Use this template:

```markdown
---
last-updated: YYYY-MM-DD
---

# <Workstation Name> - Workstation Rules

## Identity

One paragraph: who this workstation is, what routes here, what doesn't.

## Resources

| Resource | Read when... |
|---|---|
| `00_Resources/voice-principles.md` | Before drafting any content |
| (add workstation-specific resources as you create them) | ... |

## Workflow

1. <First step>
2. <Second step>
3. **STOP** if user approval is needed before continuing.
4. <...>

## Editorial Rules

Follow my voice principles in `00_Resources/voice-principles.md`. Layered rules:

- <rule 1>
- <rule 2>

## Failure modes

- <failure 1>
- <failure 2>
```

### 3. Create `MEMORY.md`

```markdown
# <Workstation Name> Memory

## Contacts
_(empty)_

## Key Decisions
_(empty)_

## Recent Activity
_(empty)_
```

### 4. Add the workstation to root `CLAUDE.md`

In the **Routing Map** section, add a row:

```
| <Workstation Name> | I need to <do the thing this workstation handles> |
```

### 5. Add to `00_Resources/index.md`

Append the workstation to the topic index.

### 6. Test it

Open a session. Trigger the workstation by saying something that should route there. Confirm the agent loads the workstation `CLAUDE.md`. If it doesn't, your `Identity` section isn't specific enough about routing triggers - revise.

## Anti-patterns

- **Pre-emptive workstations.** Don't create a workstation "in case you need it later." Empty workstations rot. Create when the actual need shows up.
- **Workstation per domain.** A workstation isn't a topic - it's a workflow. "Investing" isn't a workstation; it's a `LifeOS/` sub-folder. "Investing Research" *could* be a workstation if it has a distinct workflow (e.g., a 5-step research-and-decision flow).
- **Mega-workstation.** If a workstation's `CLAUDE.md` is over 200 lines, it's doing too much. Split into 2 workstations with clear routing.

## When to retire a workstation

Retire when:
- You haven't routed there in 60+ days
- The workflow has been absorbed into a different workstation
- You've automated the work so the workstation is no longer needed

To retire: move the workstation folder to `_archive/workstations/<date>/<name>/`. Update the Routing Map and index.md. The agent stops loading it.
