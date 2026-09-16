# Change records

The audit trail of material change. One file per change request, named `CR-001.md`,
`CR-002.md`, and so on. Copy [`CR-TEMPLATE.md`](CR-TEMPLATE.md) to start one.

## What belongs here

Any stakeholder feedback that alters the business or solution contract. Cosmetic
implementation refinement does **not** need a CR — it stays in `plan.md`.

The test: *if this change were made silently, would someone who signed `Intent.md`
or `Spec.md` be surprised?* If yes, it needs a record.

## Why records exist

So that six months later the question "why does this system recommend maintenance
actions when the approved Intent said it wouldn't?" has an answer — either a CR
that shows who approved the expansion, or the absence of one, which shows how the
drift happened.

## The one rule

`change-reconciliation` classifies, identifies ownership and notifies. **It cannot
approve.** Sections 5–7 of every CR require a human. An agent that approves its own
scope expansion is not a governance system.

## Index

| CR | Title | Classification | Status | Affected | Decision |
|---|---|---|---|---|---|
| | | | | | |
