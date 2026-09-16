---
artifact: change-request
id: CR-000
status: proposed        # proposed | classified | routed | approved | deferred | rejected | implemented
classification:         # A | B | C | D
raised_date:
raised_by:
owner:                  # team accountable for reassessment
approver:               # who signs the resulting artifact change
---

# CR-000 — <short title>

## 1. Feedback as received

> Record the stakeholder's actual words. Do not pre-interpret. The raw request
> is evidence; the interpretation is analysis, and they belong in different
> sections.

**Raised by:** <name, role>
**Context:** <demo, review session, written feedback>

## 2. Classification

| | Classification | Selected |
|---|---|---|
| **A** | Implementation refinement — no business or solution meaning changes | |
| **B** | Technical blueprint change — Spec reassessment + technical re-sign | |
| **C** | Business / scope change — Intent reassessment + business re-sign, **then** Spec reassessment | |
| **D** | Future scope — change record / backlog only | |

### Rationale

<!-- Why this classification and not the adjacent one. The reasoning matters
     more than the label, because the label is what gets argued about later. -->

### Reference tests

| Signal | Reading |
|---|---|
| Does any Intent §7 scope item change? | → C |
| Does a new persona, decision authority or business outcome appear? | → C |
| Does system behavior, architecture, data or platform choice change? | → B |
| Is only presentation, wording or local implementation affected? | → A |
| Is it valuable but not now? | → D |

> The reconciliation agent **proposes** this classification. A human accepts it.

## 3. Affected artifacts

| Artifact | IDs affected | Change required |
|---|---|---|
| `Intent.md` | | |
| `Spec.md` | | |
| `recipes/` | | |
| `evals/` | | |
| `traceability.yaml` | | |

### Downstream regeneration

<!-- For classification B and C: what becomes stale and must be regenerated
     after re-approval. -->

- [ ]

## 4. Routing

| | |
|---|---|
| **Owning team** | |
| **Notified on** | |
| **Reassessment requested** | |
| **Reassessment complete** | |

## 5. Decision

**Decision:** accept now / defer / reject

### Rationale

### If deferred

| Revisit when | Recorded where | Owner |
|---|---|---|
| | | |

## 6. Resulting version change

| Artifact | From | To | Re-signed by | Date |
|---|---|---|---|---|
| `Intent.md` | | | | |
| `Spec.md` | | | | |

## 7. Approval

> An agent may fill sections 1–4. Sections 5–7 require a human.

| Role | Name | Date | Decision |
|---|---|---|---|
| Classification accepted by | | | |
| Artifact owner | | | |
| Re-sign authority | | | |
