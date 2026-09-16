# Golden thread — one problem, end to end

**Read this first.** It is the fastest way to understand the system.

One equipment-reliability use case, from a vague idea to a governed artifact chain. The
domain does not matter. What matters is watching a one-sentence idea become something
buildable, and watching a helpful demo comment get stopped before it became unapproved
scope.

## The journey

| Stage | What happened |
|---|---|
| **Vague idea** | "Use AI to identify equipment reliability risks." |
| **`grill-me`** | Who uses it? Which process? What signals? What decision? What value? |
| **Intent** | Approved scope: identify leading risk and reconstruct evidence — **not** prescribe maintenance. |
| **`intent-roast`** | Found a weak value statement. "Improve reliability" became a measurable outcome with a baseline, a target and a decision owner. |
| **Spec + reuse** | Reuse retrieval, data quality and tooling. Create only the genuinely novel chronology capability. |
| **`architecture-roast`** | Found an unnecessary agent. A deterministic timestamp transformation was removed from the agent graph and implemented as a tool. |
| **Prototype** | Agents built from approved Spec, recipes, context and eval criteria. |
| **Feedback** | *"Add maintenance recommendation."* Classified **C** — scope expansion → CR → defer or amend Intent. |
| **`change-reconciliation`** | Found prototype scope drift. The recommendation did not slip into code; it became an explicit business decision. |

## The three catches

These are the whole point of the playbook, and each one was caught by a different
mechanism.

**A weak value statement, caught by challenge.** "Improve reliability" cannot fail, so it
cannot be proven. It became: *14 unplanned failures per quarter across 200 monitored
assets → ≤ 10 within two quarters, measured by the same maintenance-system query, owned by
the Reliability Engineering Manager.* Now the project can be judged.

**An unnecessary agent, caught by design review.** `NormalizationAgent` was justified as
"handles varied timestamp formats." That is parsing. As an agent it added
non-determinism, latency and cost to a step that should have unit tests. It became
`timestamp_normalize`, a tool.

**Scope drift, caught by classification.** *"Can we also recommend the maintenance
action?"* — one sentence, said helpfully, in a demo. Intent §7 explicitly excluded
prescription. Without classification it becomes a ticket, then a feature, then a system
doing something no business owner approved, with no record of when the decision was made.
Classified **C**, it became a decision the business owner made explicitly.

## Files

| File | Shows |
|---|---|
| [`Intent.md`](Intent.md) | An approved Intent with a real baseline, a populated out-of-scope section, and sign-off |
| [`Spec.md`](Spec.md) | The coverage matrix, agent justifications, the demoted component, reuse decisions, negative evals |
| [`traceability.yaml`](traceability.yaml) | `INT-004 → SPEC-011 → recipe → EVAL-007`, plus the CR impact record |
| [`CR-001.md`](CR-001.md) | The scope-expansion classification, with the full cascade priced out |

## How to use it

Read `Intent.md` §6 and §7 first — the value hypothesis and the out-of-scope section. Those
two are what the rest of the chain depends on, and they are the two most often written
badly.

Then read `CR-001.md`. It shows what §7 was *for*.
