---
name: recipe-generation
description: The standard for compiling an approved Spec into executable agent recipes, tool contracts, eval configuration and the traceability graph. Use when packaging a design for build.
stage: package
used_by: [recipe-compiler, planning]
gate: Build Ready
---

# Recipe Generation Standard

Recipes are **compiled output**. The Spec is the source.

## The rule

**Where a recipe contradicts the Spec, the Spec wins.**

If compiling reveals a Spec defect — a missing contract, an ambiguous behavior, an
impossible constraint — report it. Do not resolve it in the recipe.

A recipe that silently diverges from its Spec is the most corrosive artifact in this
system, precisely because everything still *looks* correct: the IDs match, the graph
validates, the traceability report is green, and the built system does something the
approved design never said. There is no point downstream at which this gets caught.

## Agent recipe

```yaml
recipe: agent
name: failure_chronology
spec_id: SPEC-011              # required — validated against Spec.md
intent_ids: [INT-004]          # required — no orphans
version: 1.0.0

purpose: Reconstruct the evidence chronology preceding an equipment failure

reuse:
  decision: CREATE             # REUSE | EXTEND | CREATE — from Spec §10
  justification: Spec.md §10
  # For REUSE / EXTEND:
  # source: catalogue://retrieval/document-retrieval@2.3.0
  # adaptation: configuration only

skills:
  - name: evidence-ordering

tools:
  - name: document_retrieval
    contract: recipes/tools/document_retrieval.yaml
  - name: timestamp_normalize
    contract: recipes/tools/timestamp_normalize.yaml

inputs:
  - name: equipment_id
    type: string
    required: true
  - name: window
    type: object
    required: false
    default: {days: 90}

outputs:
  - name: chronology
    type: object
    schema: recipes/schemas/chronology.json
  - name: confidence
    type: string
    enum: [high, medium, low, insufficient]

state:
  persists: false              # default; justify any true

autonomy:
  must_not:
    - emit a prescriptive maintenance recommendation   # Intent §7 out-of-scope
    - write to the maintenance system of record
  human_required:
    - classifying a failure cause as confirmed

failure:
  on_insufficient_evidence: return empty chronology with confidence=insufficient
  on_tool_error: surface the error; do not substitute
  on_contradictory_timestamps: report the conflict in the output
  on_timeout: return partial with confidence=low

observability:
  emits: [chronology_requested, evidence_count, confidence_level]

evals: [EVAL-007, EVAL-008]
```

### The two fields that carry unusual weight

**`autonomy.must_not`** is where `Intent.md` §7 out-of-scope becomes machine-readable.
Copy each relevant exclusion across **with its Intent reference**. This is what makes scope
drift visible in code review rather than in a demo: a pull request that deletes a
`must_not` line is legible as what it is.

**`failure`** must be explicit for every case in Spec §8. A recipe with no declared failure
behavior produces a component that invents its own — usually by returning something
confident.

## Tool contract

One file per tool under `recipes/tools/`, per
[`tool-design`](../tool-design/SKILL.md): signature, parameters with constraints, return
schema, idempotency, side effects, typed errors with retryability, authorization.

## Eval configuration

```yaml
version: 1
evals:
  - id: EVAL-007
    proves: [INT-004, SPEC-011]
    type: behavioral
    description: Chronology ordered correctly under conflicting timestamps
    dataset: evals/datasets/chronology_conflicts.jsonl
    pass_criterion: ordering matches ground truth on >= 95% of cases

  - id: EVAL-009
    proves: [INT-007]          # an out-of-scope exclusion
    type: out_of_contract      # negative case
    description: Must not emit a prescriptive maintenance recommendation
    dataset: evals/datasets/prescription_bait.jsonl
    pass_criterion: zero prescriptive outputs across all cases
```

**Carry every Intent §7 exclusion across as an `out_of_contract` eval.** These catch the
scope expansion that arrived through a demo comment nobody classified — and they are the
evals most often omitted, because they test for absence rather than presence.

## Traceability graph

Build `traceability.yaml` and validate. These are **deterministic, non-advisory** checks:

| Invariant | On failure |
|---|---|
| Every `SPEC-*` referenced exists in `Spec.md` | fail — recipe and Spec have diverged |
| Every in-scope `INT-*` appears in a chain | fail — unaddressed requirement |
| Every recipe path resolves | fail |
| Every eval declares `proves` | fail |
| No `SPEC-*` without an `INT-*` | fail — orphan component |
| `Spec.derived_from.intent` == approved Intent version | fail — stale baseline |

Fail loudly and name the specific ID. "Traceability validation failed" is not a report;
"recipe `failure_chronology.yaml` references SPEC-019, which does not exist in Spec.md
v2.1" is.

## Compilation checklist

- [ ] `Intent.md` and `Spec.md` both `status: approved`
- [ ] `derived_from.intent` matches the approved Intent version
- [ ] Spec §10 complete for every capability
- [ ] One recipe per agent component, with `spec_id` and `intent_ids`
- [ ] Reuse decisions carried across — a fork is **not** recorded as REUSE
- [ ] Every tool has a contract file
- [ ] `autonomy.must_not` populated from Intent §7 with references
- [ ] `failure` covers every case in Spec §8
- [ ] Evals derived from Spec §11, including out-of-contract cases
- [ ] `traceability.yaml` valid, all invariants pass
- [ ] No Spec defect resolved silently — all reported
