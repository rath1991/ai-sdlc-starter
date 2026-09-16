# Gates

A gate checks that a defined contract is sufficiently complete, consistent and approved
before work advances.

## The enforcement principle

Use the cheapest reliable control.

| Level | Mechanism | For |
|---|---|---|
| **1 — Guidance** | Skills encode institutional knowledge | Things that should usually happen |
| **2 — Agentic validation** | Independent semantic review | Judgment: is this complete, over-engineered, drifting? |
| **3 — Deterministic gates** | Machine-checkable invariants | Facts that must never be optional |

Every check below is marked **semantic** or **deterministic**. Deterministic checks are
machine-checkable and are not advisory — they either pass or block. Semantic checks require
a reviewing agent or a human, and their finding is a judgment that can be accepted with
documented reasoning.

## The four gates

| Gate | Advances from | Owner |
|---|---|---|
| [Intent Ready](intent-ready.md) | Intent draft → business review → approved | Business blueprint authority |
| [Design Ready](design-ready.md) | Spec draft → technical review → approved | Technical blueprint authority |
| [Build Ready](build-ready.md) | Approved Spec → packaged → build | Delivery, on compiler validation |
| [Change Control](change-control.md) | Feedback → classified → routed → re-signed | Owning artifact team |

## What a gate cannot do

**No agent can pass a gate that requires a sign-off.** Intent Ready and Design Ready end in
a human approval that no agent in this repository may perform, record, or work around.

An agent may report that every check passes. Only a human can approve.
