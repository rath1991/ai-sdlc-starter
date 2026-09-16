---
name: architecture-roast
description: Adversarially challenge a draft Spec. Attacks over-agentization, unclear ownership, unsupported services, state and memory design, failure behavior, data assumptions and orphan components. Use after drafting Spec.md and before technical sign-off.
priority: high
stage: blueprint
used_by: [technical-blueprint]
gate: Design Ready
---

# Architecture Roast

Attack the design. The most valuable outcome is a smaller system.

## When to invoke

After `Spec.md` is drafted, before technical review. Also after any material Spec
amendment.

## Stance

Your best finding is usually a component that should not exist.

Be specific about location and consequence. Not "too many agents" but: "SPEC-013
NormalizationAgent performs a deterministic timestamp transformation — §3 justifies it as
'handles varied formats', which is a parsing problem. As an agent this adds
non-determinism, latency and cost to a step that should be a tool with unit tests."

## The interrogation

### Why is this an agent?

Apply the test to every agent in §4:

> Given identical input twice, must this component be *permitted* to do something
> different, based on judgment?

If no — if the transformation is deterministic, rule-expressible, or a fixed sequence —
**it is a tool, not an agent.** → **BLOCKING**.

Then:

- Does §3 justify each agent's need to reason, or does it describe what it does?
- Would a deterministic implementation produce a better result — testable, cheaper,
  faster, debuggable?
- Is an agent used because it was the most capable primitive available rather than the
  right one?

### Why this many?

- Count them. Then ask what each one *decides*. An agent that decides nothing is a
  function call.
- Could two adjacent agents be one? Chained agents passing structured data to each other
  often indicate a pipeline that was modelled as a conversation.
- Is there an orchestrator whose only job is calling others in fixed order? That is a
  function → **MAJOR**.
- Does agent count grow with the *problem*, or with the *design pattern*?

*Agent sprawl is the most expensive failure in this class of system.* Each agent
multiplies non-determinism, cost, latency and debugging difficulty. Three agents that each
work 95% of the time compose to something that works 86% of the time, and the failure is
harder to locate than in any of them alone.

### Does every component trace to Intent?

- Walk §1 coverage. Is every in-scope `INT-*` addressed? → **BLOCKING** if not.
- Walk §3. Does every `SPEC-*` carry an Intent ID? An orphan means either a missing
  requirement or a component that should not exist — determine which → **BLOCKING**.
- Is anything designed for a persona who does not appear in Intent?
- Is anything designed for a scope item in Intent §7 **out**-of-scope? → **BLOCKING**.

### Who owns what?

- For each component: who operates it, who is paged, who owns the data it reads?
- Where does the boundary sit between this system and systems it integrates with?
- Is any component owned by "the platform team" without that team having agreed?
- Are two components able to write the same state? → **MAJOR** at minimum.

### Is the state design deliberate?

- What persists, where, for how long, and why?
- If an agent has memory: what is remembered, what is the retention, what happens to a
  stale memory that is now wrong?
- Is memory used where a parameter would do? Conversational state is the most common
  unnecessary complexity in agent designs.
- Who can read the state? Does it contain anything whose classification forbids that?
- What happens on concurrent access?

### What happens when it fails?

- Does §8 cover every component, or only the interesting ones? → **MAJOR** if partial.
- For each: detection, response, user-visible behavior.
- Is there a degradation policy? Missing → **BLOCKING**.
- **Does the system ever degrade into confident wrong answers?** → **BLOCKING**. For a
  decision-support system this is worse than stopping, and worse than having no system.
- Is "return nothing, and say why" available as a response? If every path must produce an
  answer, the design has no honest failure mode.
- What happens when a tool errors? Does the agent retry, substitute, or surface it?
  Substitution → **BLOCKING**.

### Are the data assumptions stated and survivable?

- Does §6 state assumptions about completeness, timeliness and accuracy?
- For each: what happens if violated, how is it detected? Unstated → **BLOCKING**.
- Has anyone looked at the actual data, or is this designed against the schema?
- Does the design assume clean historical data? *Most systems of this kind that fail in
  production fail here, not in the model.* The demo works because the demo data is clean.
- Who owns data quality, and have they agreed to the assumption?

### Are the platform choices approved?

- Is every service in the approved set? → **BLOCKING** if not, unless there is an
  exception request with a named approver.
- Are integration patterns the approved ones?
- Identity, network boundaries, residency, retention — addressed?
- Are responsible-AI requirements addressed, or referenced and left?

### Do the evals prove the promises?

- Does every Intent promise have an eval, or a stated reason it is untestable?
- Does every eval declare what it proves?
- Is there an out-of-contract table? Missing → **MAJOR**.
- **Is each Intent §7 exclusion represented as a negative eval?** This is what catches
  scope drift in code review rather than in a demo.
- Could every eval pass while the system is useless? If yes, the evals measure mechanics
  rather than value → **MAJOR**.

### Is this the simplest design that works?

Last, and worth asking plainly:

- What would a design with half the components look like, and what would it lose?
- Which component is here because of the pattern rather than the problem?
- If you had to delete one thing, what would it be — and why has it not been deleted?

## Output

```markdown
## Architecture Roast — <project>, Spec v<x.y>

**Verdict:** NOT READY FOR REVIEW | READY WITH FINDINGS | READY

### Blocking (n)
1. **<finding>** — SPEC-<id>, §<section>
   - Observed:
   - Consequence:
   - Recommendation:

### Major (n)
### Minor (n)

### Components recommended for removal
| SPEC ID | Component | Recommendation | Rationale |

### Simplification opportunities
### Assessed and sound
```

## Calibration

Reserve **BLOCKING** for: an unjustified agent, an orphan component, an unaddressed
in-scope item, a missing degradation policy, silent confident failure, an unstated data
assumption, an unapproved service, designed-for out-of-scope behavior.

Name what is sound. An architect reading a list of fourteen undifferentiated complaints
cannot tell which two matter.
