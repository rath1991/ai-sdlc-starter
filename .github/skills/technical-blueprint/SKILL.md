---
name: technical-blueprint
description: The standard for producing Spec.md from an approved Intent — structure, required sections, altitude, and what makes a Spec buildable and defensible. Use when drafting or reviewing a Spec.
stage: blueprint
used_by: [technical-blueprint, reusable-qualification]
gate: Design Ready
---

# Technical Blueprint Standard

How to produce a `Spec.md` that a team can build from and an architect can defend.

## Preconditions

`Intent.md` at `status: approved`. Record the exact version in `derived_from.intent`.

Designing against a draft Intent produces a Spec that goes stale on the next amendment,
and — worse — lends the unapproved Intent false authority, because something now depends
on it.

## The altitude rule

A Spec is not a design document and not an implementation guide. The test:

> Could a competent team build this **and** would an architect recognize what they built?

**Too high:** "The system retrieves relevant documents and reconstructs a timeline."
Nothing about ordering under conflicting timestamps, or what happens when evidence is thin.

**Too low:** function signatures, class hierarchies, prompt text. These belong in recipes
and code, where they can change without a re-sign.

**Right:** behavior, boundaries, contracts, failure modes, and the reasoning behind each —
the decisions that would be expensive to reverse.

A useful heuristic: *if changing it does not require re-approval, it does not belong in the
Spec.*

## Required sections

Nothing here is optional. An absent section is a Design Ready failure, not a gap to fill
later.

| § | Section | Must contain |
|---|---|---|
| 1 | Intent coverage | Every in-scope `INT-*` → addressing component; deliberate omissions stated |
| 2 | Solution behavior | Triggers, behavior, outputs, in plain language, each mapped to an Intent ID |
| 3 | Architecture | Components with IDs, types, responsibilities, Intent IDs — **plus** why each agent is an agent |
| 4 | Agents | Purpose, skills, tools, I/O, state, autonomy boundary, failure behavior |
| 5 | Tools | Contract, idempotency, side effects, errors, authorization |
| 6 | Data | Datasets, owners, classification, refresh — **plus** quality assumptions and violation handling |
| 7 | Enterprise constraints | Constraint, source standard, how the design complies |
| 8 | Failure behavior | Per component, plus a degradation policy |
| 9 | Observability | Signals, emitters, consumers, thresholds |
| 10 | Reuse / create | Every capability classified with justification |
| 11 | Eval criteria | Each eval names what it proves; out-of-contract behaviors included |
| 12 | Sign-off | Human only |

## Order of work

Behavior before architecture. This is not stylistic.

Architecture written first acquires components suggested by the pattern rather than
required by the problem, and those are exactly what `architecture-roast` removes — after
you have designed them in detail. Writing §2 first constrains §3 to what the behavior needs.

1. **§1 coverage matrix, empty.** The design brief. Keeps you honest both ways: nothing
   in scope forgotten, nothing designed that no requirement asked for.
2. **§2 behavior**, in plain language.
3. **§3 components**, derived from §2. Assign IDs. No orphans.
4. **§3 agent justification.** Apply the reasoning test. Expect to lose components.
5. **§4–5** agents and tools.
6. **§6 data**, including quality assumptions — the section that decides production success.
7. **§8 failure behavior** and degradation policy.
8. **§7 constraints**, §9 observability.
9. **§10** via `reusable-qualification`.
10. **§11 evals**, derived from promises, including negative cases.
11. `architecture-roast`, then hand over.

## The four things most often wrong

**Agents that should be tools.** Given identical input twice, must it be permitted to
differ, based on judgment? If no, it is a tool. This single test removes more unnecessary
complexity than anything else in the standard.

**Data assumptions unstated.** The design assumes parseable timestamps, complete history,
timely arrival. State each one, what happens when it is violated, and how you detect it.
Systems of this kind fail in production here far more often than in the model.

**No honest failure mode.** If every path must return an answer, the system will return
confident wrong answers under degradation. "Return nothing, and say why" must be available.

**Evals that measure mechanics.** If every eval could pass while the system is useless, the
evals test plumbing. Derive them from Intent promises, and include the negative cases from
Intent §7.

## Traceability obligations

- Every `SPEC-*` carries an Intent ID.
- Every in-scope `INT-*` appears in §1 as addressed or deliberately deferred.
- Every eval declares what it proves.
- `derived_from.intent` records the approved version.

These are checked deterministically at the gate. Unknown IDs and orphans fail.

## What the Spec must not do

- **Widen scope.** If the design needs something Intent excludes, route it — do not design
  it in.
- **Restate standards.** Reference them; restated standards go stale silently.
- **Contain prompt text or signatures.** Those live in recipes, and must be changeable
  without a re-sign.
- **Resolve an Intent ambiguity by choosing.** Report it.
