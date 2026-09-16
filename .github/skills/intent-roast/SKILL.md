---
name: intent-roast
description: Adversarially challenge a draft Intent before it reaches business review. Attacks value, scope, baselines, assumptions and measurability. Use after drafting Intent.md and before requesting business sign-off.
priority: high
stage: intent
used_by: [intent-synthesis]
gate: Intent Ready
---

# Intent Roast

Attack the draft. Find what a sceptical executive would find, before they find it.

## When to invoke

After `Intent.md` is drafted, before it goes to business review. Also after any material
Intent amendment.

## Stance

You are not proofreading. You are looking for the reason this project fails.

Be direct and specific. "The value hypothesis is weak" helps nobody. "§6 claims 30%
fewer unplanned outages with no current outage rate stated, no measurement method and no
named owner — none of the three exists in the cited sources" is a finding someone can act
on.

Severity:

| | Meaning |
|---|---|
| **BLOCKING** | Must be resolved before business review |
| **MAJOR** | Should be resolved; document explicitly if accepted as-is |
| **MINOR** | Worth fixing, not worth delaying for |

## The interrogation

### Why AI?

- What specifically requires judgment rather than a report, a rule or a query?
- What is the non-AI alternative, and why was it rejected?
- If the answer is "AI is strategic" → **BLOCKING**. That is sponsorship, not a rationale.
- Would a deterministic solution deliver 80% of the value at 20% of the cost? If plausibly
  yes and unaddressed → **BLOCKING**.

### Where is the baseline?

- Is there a current-state number? No → **BLOCKING**.
- Is it measured or estimated? Estimated and unlabelled → **MAJOR**.
- Where does the measurement come from, and does that source still exist?
- Can it be measured the same way after the system ships? No → **BLOCKING**, because the
  improvement will be unprovable.

### How is success measured?

- Is the target a number with a unit and a date?
- Who owns the measurement? Unnamed → **BLOCKING**.
- When is the first review?
- Could this be measured and *fail*? If no failing outcome is definable, it is not a
  measure → **BLOCKING**.
- Is the measure attributable to this system, or is it a business metric that moves for
  twelve other reasons? → **MAJOR**.

### Who signed up to this?

- Is the business owner named, and do they know?
- Is the person accountable for the outcome the same person sponsoring the project?
- Do the named personas know their workflow is expected to change?
- Who accepts the result — and have they agreed the success criteria?

### What would make this fail?

Ask it directly, then check the draft against the common answers:

- Data does not exist, or is worse than assumed → is this in §9 with a validation plan?
- Users do not adopt it → what makes adoption likely? Is anything in the draft about it?
- The decision it supports is not actually made that way → was the process validated?
- The output is not trusted → what makes it trustworthy enough to act on?
- The organization cannot act on what it learns → then the value does not land.

An Intent with no credible failure mode in §9 has not been thought about honestly →
**MAJOR**.

### Is the scope real?

- Does §7 out-of-scope exist and have content? No → **BLOCKING** *(deterministic gate check)*.
- Does out-of-scope contain the things people will actually ask for in the demo? A section
  listing only obvious exclusions is decoration → **MAJOR**.
- Is every in-scope item traceable to a stated pain point?
- Is the smallest useful version identifiable, or is this all-or-nothing?

### Are the personas real?

- Does each persona appear in the source package, or were they inferred? Inferred and
  unlabelled → **MAJOR**.
- Is the workflow change described concretely?
- Is there a persona here whose only function is to justify a component?
- Whose workflow does *not* change? If nobody's does, → **BLOCKING**.

### Is the evidence real?

- Does every §1 claim cite a source?
- Are `Inferred` claims labelled as inferred?
- Do any two sources contradict each other, and is the contradiction recorded rather than
  resolved?
- Is any cited source undated, unattributed or marked draft, and treated as authoritative?

### Is the problem the real problem?

The hardest question, and worth asking last:

- Is this the problem, or a symptom of one upstream?
- Would fixing the upstream process remove the need for this system entirely?
- Is this solving a problem the organization has, or one the technology is suited to?

## Output

```markdown
## Intent Roast — <project>, Intent v<x.y>

**Verdict:** NOT READY FOR REVIEW | READY WITH FINDINGS | READY

### Blocking (n)
1. **<finding>** — §<section>
   - Observed:
   - Why blocking:
   - To resolve:
   - Who can answer:

### Major (n)
### Minor (n)

### Routed to business blueprinting
| Question | Section | Blocking |

### Assessed and sound
<!-- Name what holds up. A roast that finds only problems reads as noise,
     and the reviewer loses the ability to tell severity apart. -->
```

## Calibration

A roast that blocks on everything is as useless as one that blocks on nothing. Reserve
**BLOCKING** for what genuinely cannot proceed: no baseline, no measurement owner, empty
out-of-scope, unresolved contradiction in a scope item, no definable failure.

And say what is sound. The reviewer needs to know the difference between a draft with
three real problems and a draft that is broadly wrong.
