---
name: process-mapping
description: Map the current-state process precisely enough to locate where the problem occurs and which decision the system supports. Use when synthesizing Intent from a blueprint package, or when feedback suggests the process was misunderstood.
stage: intent
used_by: [intent-synthesis, change-reconciliation]
---

# Process Mapping

Map the as-is process to the level where the problem becomes locatable.

## When to invoke

- Drafting `Intent.md` §2 from a blueprint package.
- The supplied process map is high-level and the problem is not visible in it.
- Feedback suggests the process was misunderstood.

## Why this matters more than it looks

A system designed against a process nobody actually follows is the most expensive kind of
correct. The mapped process in the workshop deck is frequently the *intended* process; the
real one has a workaround in step four that someone built in a spreadsheet three years ago,
and that workaround is where the problem lives.

## What to map

### The spine

| Step | Actor | Trigger | Inputs (source, condition) | Decision / output | Duration | Pain |
|---|---|---|---|---|---|---|

Map only the span that contains the problem. A twelve-step end-to-end map where the problem
lives in steps 4–6 has buried the signal.

### The decision points

For each step where someone decides something:

- What is being decided?
- On what information?
- What does the decision-maker *wish* they had?
- What happens when the information is missing or late? Do they wait, or guess?
- Is the decision reversible? What does a wrong one cost?

**The decision the system supports must be identifiable here.** If no step in the map
contains a decision the system would improve, either the map is at the wrong altitude or
the use case does not target a decision — and that is a finding for `intent-roast`.

### The information flow

- Where does each input originate? Which system, which person?
- In what condition does it arrive — structured, free text, a PDF, verbal?
- Is it complete and timely, or is a step routinely done with partial information?
- What transformation happens between systems, and does anyone do it by hand?

### The workarounds

Ask directly: *what are people doing today to cope?*

The spreadsheet, the saved query, the person who knows which records to distrust, the
recurring meeting that exists to reconcile two systems. Each one is unpaid documentation
of exactly where the process fails, and it is usually absent from the official map.

A workaround also tells you what good looks like: someone has already built the minimum
viable version of your system, by hand, and their version has requirements in it.

### The handoffs

Where work passes between people, teams or systems:

- What is lost? Context, urgency, rationale?
- How long does the handoff take relative to the work?
- Does the receiver have to reconstruct something the sender already knew?

Handoffs are where delay and information loss concentrate, and where a system that merely
reconstructs evidence can deliver value without changing anyone's decision authority.

## Depth test

Map deeper when:

- The step contains the problem or the decision the system supports.
- A workaround exists.
- Information crosses a system or team boundary.
- The pain point is reported here.

Stop when:

- Further detail does not change what the system would do.
- You are documenting a process the system does not touch.

## Output

Populate `Intent.md` §2, plus:

```markdown
### Process findings

**Decision the system supports:** <step, actor, decision>

**Where the problem occurs:** <step(s)>, with evidence

**Workarounds in use:**
| Workaround | Who | What it compensates for | What it implies we need |

**Information gaps:**
| Step | Missing / late input | Current coping behavior | Consequence |

**Handoff losses:**
| Handoff | What is lost | Cost |

**Unverified in the source package:**
<!-- Steps taken from documentation without confirmation. Mark clearly —
     these are assumptions, and they belong in Intent §9. -->
```

## What not to do

**Do not map the intended process and call it current state.** If the package describes
the designed process, label it as such and record that the actual process is unverified.

**Do not smooth over the ugly parts.** The step where three people meet weekly to reconcile
two systems by hand is not an embarrassing detail to tidy up. It is the requirement.
