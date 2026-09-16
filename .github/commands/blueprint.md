# /blueprint

Create the technical `Spec.md` from the approved Intent.

**Agent:** `technical-blueprint` · **Gate:** Design Ready
**Requires:** `Intent.md` at `status: approved`

## Precondition

Check `Intent.md` frontmatter. If it is not `approved`, **stop** and report the status
found. Designing against a draft Intent produces a Spec that goes stale on the next
amendment.

Record the exact approved version in `derived_from.intent`.

## Steps

Follow [`.github/agents/technical-blueprint.agent.md`](../agents/technical-blueprint.agent.md)
and [`.github/skills/technical-blueprint/SKILL.md`](../skills/technical-blueprint/SKILL.md).

Order matters — **behavior before architecture**:

1. §1 coverage matrix, empty. The design brief.
2. §2 behavior in plain language, mapped to Intent IDs.
3. §3 components with `SPEC-*` IDs. No orphans.
4. §3 agent justification — apply the reasoning test. Expect to lose components.
5. §4–5 agents (`agent-design`) and tools (`tool-design`).
6. §6 data, **including quality assumptions and violation handling**.
7. §8 failure behavior per component, plus the degradation policy.
8. §7 constraints, §9 observability.
9. §11 evals derived from promises, including out-of-contract cases.

## Rules

- Never widen scope. If the design needs something Intent excludes, route it.
- Never resolve an Intent ambiguity by choosing. Report it.
- Reference standards; do not restate them.
- No prompt text or signatures — those live in recipes.

Set `status: draft`, then run `/roast-spec`.
