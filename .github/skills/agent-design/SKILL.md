---
name: agent-design
description: The standard for designing an agent component — when something should be an agent at all, and how to specify purpose, autonomy boundary, state, tools and failure behavior. Use when a Spec component is proposed as an agent.
stage: blueprint
used_by: [technical-blueprint, reusable-qualification, recipe-compiler]
---

# Agent Design Standard

## First, the question that comes before design

> Given identical input twice, must this component be **permitted** to do something
> different, based on judgment?

**No** → it is a tool. Stop here and use [`tool-design`](../tool-design/SKILL.md).

**Yes** → continue, and be able to say what the judgment is.

This test is not a formality. Every agent introduced multiplies non-determinism, cost,
latency and debugging difficulty. Three agents at 95% reliability compose to roughly 86%,
and locating the failure is harder than in any one of them. The right count is the smallest
number that covers the actual judgment in the problem.

Symptoms of a component that should not be an agent:

- The justification describes *what it does* rather than *what it decides*.
- It transforms structured input into structured output by fixed rules.
- Its behavior is "handles varied formats" — that is parsing.
- It calls other components in a fixed order — that is a function.
- You cannot name a case where two runs should legitimately differ.

## Required specification

### Purpose

One sentence: the judgment this agent makes. If the sentence describes a transformation
rather than a decision, return to the test above.

### Invocation

What triggers it, what calls it, whether it is user-facing or internal.

### Skills

Which skills it invokes, and when. An agent with no skills is usually a prompt with
ambitions — ask what institutional knowledge it is applying.

### Tools

Each tool with its contract. Tools are the deterministic layer: **anything that can be a
tool should be.** The agent's job is choosing and interpreting, not computing.

### Inputs and outputs

Names, types, schemas, required or optional. Outputs should be structured enough that a
downstream component does not have to parse prose.

### State and memory

The section most often wrong.

| Question | Why it matters |
|---|---|
| What persists? | Everything else is per-invocation |
| Where, for how long? | Determines classification and retention obligations |
| What happens when a memory is stale and now wrong? | Most designs have no answer |
| Who can read it? | Classification may forbid the obvious choice |
| Concurrent access? | Two invocations writing the same state |

**Default to stateless.** Conversational memory is the most common unnecessary complexity
in agent design. If a parameter would do, use a parameter. Memory that exists because the
framework offers it will eventually serve a stale answer with confidence.

### Autonomy boundary

What this agent must **never** decide alone. Derive it from `Intent.md` §7 out-of-scope and
from the human decisions Intent reserves.

Write it as explicit prohibitions, each carrying its Intent reference:

```yaml
autonomy:
  must_not:
    - emit a prescriptive maintenance recommendation   # Intent §7 out-of-scope
    - write to the maintenance system of record
    - decide an asset is safe to operate
```

This becomes `autonomy.must_not` in the recipe, which is what makes scope drift visible in
code review rather than in a demo six weeks later.

### Failure behavior

| Case | Must specify |
|---|---|
| Insufficient evidence | What it returns. **"Nothing, with a reason" is correct** more often than designers expect |
| Tool error | Surface it — **never substitute a guess** |
| Low confidence | Threshold, and behavior below it |
| Timeout | Partial result or none; which |
| Contradictory input | Report the contradiction; do not resolve silently |

The unacceptable design is one that degrades into confident wrong answers. For
decision-support that is worse than stopping, and worse than having no system.

### Evals

Which evals prove this agent works — including the negative cases from its autonomy
boundary. "Must not emit a prescriptive recommendation" is an executable check.

## Composition

When more than one agent is genuinely needed:

- **Single responsibility.** One agent, one kind of judgment.
- **Structured handoffs.** Agents pass typed data, not conversation.
- **No agent-to-agent chat loops.** If two agents need to negotiate, the decomposition is
  wrong.
- **Deterministic orchestration where possible.** If the call order is fixed, the
  orchestrator is a function.
- **One owner per piece of state.**

## Review checklist

- [ ] The reasoning test is answered, with a named judgment
- [ ] Purpose describes a decision, not a transformation
- [ ] Everything that could be a tool is a tool
- [ ] Stateless, or persistence justified with retention and staleness handling
- [ ] Autonomy boundary written as prohibitions with Intent references
- [ ] Failure behavior covers all five cases
- [ ] Never substitutes on tool error
- [ ] Can return nothing, with a reason
- [ ] Evals include the negative cases
- [ ] Traces to an Intent ID
