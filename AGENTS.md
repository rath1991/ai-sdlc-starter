# AGENTS.md

Instructions for any AI assistant working in this repository.

This is a **governed delivery workspace**, not a general codebase. Business intent and
solution design are versioned, signed artifacts. Read this before making changes.

---

## The three rules

**1. Authority flows one way.**

```
Intent.md  →  Spec.md  →  recipes/ + evals/  →  code
```

A recipe that contradicts `Spec.md` loses — the Spec wins. A Spec that contradicts
`Intent.md` means either the Intent is amended through the change process or the Spec is
corrected. **Never resolve the conflict by editing upward.**

**2. You cannot approve anything.**

No agent may set `status: approved`, fill a sign-off section (`Intent.md` §11, `Spec.md`
§12), or complete `changes/CR-*.md` sections 5–7. These require a named human. If asked,
decline and name the owning role.

**3. Material change goes through reconciliation.**

If a task requires changing what the system *means* — not how it is built — stop and run
`/reconcile-feedback`. Do not edit `Intent.md` or `Spec.md` directly to accommodate an
implementation.

---

## Before you start

Check artifact status in the frontmatter:

| File | If not `approved` |
|---|---|
| `Intent.md` | Do not write `Spec.md`. Report the status found. |
| `Spec.md` | Do not compile recipes or plan a build. |

And check staleness: if `Spec.md` `derived_from.intent` is behind the approved `Intent.md`
version, **the Spec is stale.** It needs reassessment before anything is generated from it.

---

## The agents

Six agents in [`.github/agents/`](.github/agents/). Each owns one transformation.

| Agent | Consumes | Produces | Cannot |
|---|---|---|---|
| [`intent-synthesis`](.github/agents/intent-synthesis.agent.md) | Blueprint package | `Intent.md` | Approve; invent a baseline; resolve a contradiction |
| [`technical-blueprint`](.github/agents/technical-blueprint.agent.md) | Approved `Intent.md` | `Spec.md` | Edit Intent; widen scope |
| [`reusable-qualification`](.github/agents/reusable-qualification.agent.md) | Approved `Spec.md` | Capability map (§10) | Record a fork as REUSE |
| [`recipe-compiler`](.github/agents/recipe-compiler.agent.md) | Approved `Spec.md` + §10 | `recipes/`, `evals/`, `traceability.yaml` | Resolve a Spec defect silently |
| [`planning`](.github/agents/planning.agent.md) | Build Ready package | `plan.md` + build | Change Spec meaning to make an eval pass |
| [`change-reconciliation`](.github/agents/change-reconciliation.agent.md) | Stakeholder feedback | `changes/CR-*.md` | **Approve anything** |

## The skills

Nine skills in [`.github/skills/`](.github/skills/). Invoke them; do not re-derive their
content.

**Challenge skills** — [`grill-me`](.github/skills/grill-me/SKILL.md) ·
[`intent-roast`](.github/skills/intent-roast/SKILL.md) ·
[`architecture-roast`](.github/skills/architecture-roast/SKILL.md)

**Design skills** — [`process-mapping`](.github/skills/process-mapping/SKILL.md) ·
[`value-realization`](.github/skills/value-realization/SKILL.md) ·
[`technical-blueprint`](.github/skills/technical-blueprint/SKILL.md) ·
[`agent-design`](.github/skills/agent-design/SKILL.md) ·
[`tool-design`](.github/skills/tool-design/SKILL.md) ·
[`recipe-generation`](.github/skills/recipe-generation/SKILL.md)

## The gates

Four gates in [`gates/`](gates/). Checks are marked **deterministic** (machine-checkable,
pass or block) or **semantic** (judgment, acceptable with documented reasoning).

[Intent Ready](gates/intent-ready.md) · [Design Ready](gates/design-ready.md) ·
[Build Ready](gates/build-ready.md) · [Change Control](gates/change-control.md)

## The commands

Eight commands in [`.github/commands/`](.github/commands/) — the human-facing surface.
Users should not need to know which skill you invoked.

```
/intake-blueprint → /intent-from-blueprint → /roast-intent
   → [BUSINESS SIGN-OFF]  → /blueprint → /roast-spec
      → [TECHNICAL SIGN-OFF]  → /package → /plan
         → /reconcile-feedback
```

---

## File-level rules

| Path | Rule |
|---|---|
| `business-blueprint/` | **Read-only.** Source evidence. Never edit. |
| `Intent.md` | Business truth. Only via `intent-synthesis` or an approved CR. |
| `Spec.md` | Solution truth. Only via `technical-blueprint` or an approved CR. |
| `plan.md` | Free to change. No governance authority. |
| `changes/` | §1–4 may be drafted by an agent. §5–7 human only. |
| `recipes/`, `evals/` | Compiled output. Regenerate; do not hand-edit to change meaning. |
| `traceability.yaml` | Generated. Validate, do not patch to make checks pass. |
| `project_context/` | Project-local context. Safe to update. |
| `gates/`, `.github/skills/`, `.github/agents/` | The operating model. Change deliberately. |

---

## Things not to do

- **Do not invent a baseline number.** Absent → record it as absent and blocking.
- **Do not resolve a source contradiction** by picking the more convenient reading. Record
  both, cite both, raise it.
- **Do not leave out-of-scope empty.** It is a deterministic gate failure, and it is what
  makes demo feedback answerable.
- **Do not make something an agent** when identical input need not produce different
  output. It is a tool.
- **Do not let a tool return empty in place of an error.** An agent receiving `[]` from a
  failed retrieval concludes there is no evidence and proceeds confidently.
- **Do not patch `traceability.yaml`** to make validation pass. A failure means the recipes
  and the Spec have diverged.
- **Do not report a step complete with failing evals.** Say which fail and what you
  diagnosed.
- **Do not claim a gate passed** without running its checks and reporting each one.

---

## Conventions

- `INT-*` Intent requirements · `SPEC-*` Spec components · `EVAL-*` evals · `CR-*` changes.
- Every `SPEC-*` traces to an `INT-*`. No orphans.
- Every eval declares what it proves.
- Intent §7 out-of-scope exclusions become `autonomy.must_not` entries in recipes, with
  their Intent reference, and `out_of_contract` evals.
- Record project-specific organizational terms in
  [`project_context/local_context.md`](project_context/local_context.md). This repository
  stays neutral about them.

Full operating model: [`PLAYBOOK.md`](PLAYBOOK.md).
