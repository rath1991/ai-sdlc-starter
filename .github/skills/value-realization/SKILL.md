---
name: value-realization
description: Turn a value claim into a measurable hypothesis with a baseline, target, measurement method, owner and review date. Use when drafting Intent value sections or when a value claim cannot be evaluated.
stage: intent
used_by: [intent-synthesis, change-reconciliation]
---

# Value Realization

Convert value claims into something that can be proven or disproven.

## When to invoke

- Drafting `Intent.md` §6 and §8.
- A value claim has no baseline.
- `intent-roast` blocked on measurability.
- A change request claims value and needs assessment.

## The five required elements

A value hypothesis is incomplete without all five. Four out of five is not a hypothesis.

| Element | Requirement | Absent → |
|---|---|---|
| **Baseline** | Current-state number, with its source | BLOCKING |
| **Target** | Number, unit, date | BLOCKING |
| **Measurement method** | How it is measured, repeatably, after shipping | BLOCKING |
| **Owner** | Named person accountable for measuring | BLOCKING |
| **Review date** | When the first assessment happens | MAJOR |

## Converting a claim

### Start from the decision

Value in these systems almost never comes from "insight." It comes from a decision that
gets made differently. Work backwards:

1. Which decision changes?
2. Who makes it, how often?
3. What does making it better produce — fewer failures, less downtime, earlier
   intervention, less rework?
4. What is that worth, and to whom?
5. What is the current rate of the bad outcome?

Step 5 is the baseline. If nobody can state it, you have found the real first task.

### Worked example

> "Improve equipment reliability."

Not measurable. Nothing here can fail.

**Which decision?** Whether to intervene on an asset before failure.
**Who, how often?** Reliability engineers, weekly review of ~200 assets.
**What improves?** Earlier detection → intervention before failure rather than after.
**Current rate?** Unplanned failures per quarter on the monitored population.
**Attributable?** Only if intervention is traceable to a flag the system raised.

Result:

| Element | Value |
|---|---|
| Baseline | 14 unplanned failures / quarter across 200 monitored assets (maintenance system, trailing 4 quarters) |
| Target | ≤ 10 / quarter within 2 quarters of deployment |
| Method | Same maintenance-system query, quarterly; failures classified as flagged / not flagged |
| Owner | Reliability Engineering Manager |
| Review | End of first full quarter post-deployment |

Now it can fail, which is what makes it a hypothesis.

## Three tests

### Can it fail?

If no outcome would count as failure, it is not a measure. "Improved visibility" cannot
fail. "≤ 10 unplanned failures per quarter" can.

### Is it attributable?

Can the change be attributed to this system, or is it a business metric that moves for
twelve other reasons? Prefer measures close to the system's actual output.

If only a distant metric is available, define an intermediate one you *can* attribute —
flagged-before-failure rate — and record the business metric as a longer-term indicator
with its confounders named. Do not claim the distant metric as the target.

### Will it be measurable after shipping?

Surprisingly often the baseline came from a one-off analysis nobody can repeat. Confirm
the measurement is reproducible on a schedule, by the named owner, with access they
actually have.

## When there is no baseline

Do not invent one, and do not accept the claim without one. Record it as prerequisite work:

```markdown
**Baseline: ABSENT — BLOCKING**

Required: <measure>
Source: <where it would come from>
Can it be established? <yes / no / unclear>
Effort: <estimate>
Owner: <who>
By when: <date>

Routed to: <business blueprinting team>
```

Establishing a baseline is frequently a week of analysis, and it is frequently the single
highest-value week in the project — because a project that cannot measure its baseline
cannot prove its value, and will be cancelled by whoever asks for evidence later.

## Leading and lagging

Pair them, and be honest about which is which.

| Type | Example | Attribution |
|---|---|---|
| **Leading** | Flagged-before-failure rate | Direct — the system produced it |
| **Lagging** | Unplanned failures per quarter | Indirect — depends on people acting |

Leading measures prove the system works. Lagging measures prove it mattered. A project
reporting only leading measures has not demonstrated value; one promising only lagging
measures cannot show progress for two quarters. Define both.

## Adoption is part of value

A system that works and is not used delivers nothing. If value depends on people changing
behavior, that dependency is part of the hypothesis:

- Who must change what?
- Have they agreed?
- What is the adoption measure, and who owns it?
- What happens to the value if adoption is half of what is assumed?

## Output

Populate `Intent.md` §6 and §8. Report:

```markdown
### Value assessment

**Status:** MEASURABLE | INCOMPLETE | NOT MEASURABLE

| Element | Value | Source | Status |

**Can this fail?** <yes — how / no — why that is a problem>
**Attributable?** <direct / indirect — confounders>
**Measurable post-deployment?** <yes / no — what is needed>

**Blocking gaps:**
**Adoption dependencies:**
```
