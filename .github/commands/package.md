# /package

Qualify reuse, then compile the approved Spec into an executable package.

**Agents:** `reusable-qualification`, then `recipe-compiler` · **Gate:** Build Ready
**Requires:** `Spec.md` at `status: approved`

## Phase 1 — Reuse before create

Follow [`.github/agents/reusable-qualification.agent.md`](../agents/reusable-qualification.agent.md).

1. Decompose the Spec into **capabilities**, not components — granular enough that each
   could plausibly be owned by someone else.
2. Search the approved catalogue by function, not by name. Record nulls.
3. Classify: REUSE / EXTEND / CREATE.
4. For REUSE: define the adaptation boundary and run the coupling check.
5. For CREATE: record the gap, the search, why not EXTEND, and whether it is generic
   enough to belong in the shared catalogue.
6. Populate `Spec.md` §10.

> A fork is **CREATE**, not REUSE. An all-CREATE map is a finding — redo the
> decomposition before reporting.

## Phase 2 — Compile

Follow [`.github/agents/recipe-compiler.agent.md`](../agents/recipe-compiler.agent.md)
and [`.github/skills/recipe-generation/SKILL.md`](../skills/recipe-generation/SKILL.md).

1. Verify the baseline — both artifacts approved, `derived_from.intent` current.
2. Compile agent recipes to `recipes/agents/`, with `spec_id` and `intent_ids`.
3. Compile tool contracts to `recipes/tools/`.
4. Populate `autonomy.must_not` from Intent §7, **with Intent references**.
5. Declare `failure` for every case in Spec §8.
6. Derive `evals/eval_config.yaml`, including `out_of_contract` evals.
7. Build and validate `traceability.yaml`.
8. Run Build Ready ([`gates/build-ready.md`](../../gates/build-ready.md)).

## The rule

**Where a recipe would contradict the Spec, the Spec wins.** Report Spec defects; never
resolve them in a recipe. A recipe that silently diverges still looks traceable, which is
what makes it dangerous.

Fail loudly on unknown IDs, naming the specific ID.
