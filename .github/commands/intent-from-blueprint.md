# /intent-from-blueprint

Draft a provenance-backed `Intent.md` from the indexed package.

**Agent:** `intent-synthesis` · **Skills:** `grill-me`, `process-mapping`, `value-realization`
**Requires:** `/intake-blueprint` has run

## Steps

Follow [`.github/agents/intent-synthesis.agent.md`](../agents/intent-synthesis.agent.md).

1. Extract what the package supports. Label every claim `Stated`, `Inferred` or `Absent`.
2. Map the current process (`process-mapping`) to where the problem is locatable.
3. Build the value hypothesis (`value-realization`) — baseline, target, method, owner, date.
4. Run `grill-me` on every `Absent` finding. Route what the project team can answer;
   the rest become §10 open questions.
5. Fill the template. Assign `INT-*` IDs to every in-scope item.
6. Set `status: draft`.

## Rules

- **Never invent a baseline.** Absent → `ABSENT — blocking` plus an open question.
- **Never resolve a contradiction** by picking the more convenient reading.
- **Never leave out-of-scope empty.** Populate from explicit exclusions and implied
  boundaries, then ask the business team to confirm.
- Label `Inferred` claims as inferred, so a reviewer can check the reasoning.

## Report

Draft location, claim counts by confidence, blocking gaps, open questions with routing,
contradictions carried forward.

Then run `/roast-intent`.
