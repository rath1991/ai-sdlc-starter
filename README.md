# AI-Native SDLC — Starter Repository

A working repository for running an AI-assisted software delivery lifecycle where
**business intent and solution design are governed artifacts**, not chat history.

> Do not hand teams a methodology document. Give them a repository that already
> contains the agents, skills, templates and gates.

---

## The core idea

Most AI-assisted delivery fails in one of two ways. Either the AI is handed a vague
prompt and invents scope, or the methodology exists only as a slide deck nobody opens.

This repository takes a different position:

1. **A small human contract, generated downstream.** Three artifacts carry meaning —
   `Intent.md` (business truth), `Spec.md` (solution truth), `plan.md` (execution).
   Recipes, evals and traceability are generated from them.
2. **Skills teach, agents orchestrate, gates enforce.** Three distinct primitives,
   which is what keeps an asset library from degenerating into dozens of bespoke bots.
3. **Feedback is classified before it becomes scope.** Every material demo comment is
   routed to the artifact and the owner it actually affects.

Read [`PLAYBOOK.md`](PLAYBOOK.md) for the full operating model.

---

## Repository map

| Path | Role |
|---|---|
| `Intent.md` | Business truth. Problem, personas, value hypothesis, scope, measures. Canonical. |
| `Spec.md` | Solution truth. Behavior, architecture, agents, tools, data, eval criteria. Canonical. |
| `plan.md` | Engineering plan for the current build. Execution, not governance. |
| `changes/` | `CR-*.md` records. The audit trail of material change. |
| `traceability.yaml` | Machine-readable Intent → Spec → Recipe → Eval chain. |
| `business-blueprint/` | Supplied upstream source evidence. Read-only input, never edited here. |
| `project_context/` | Project-local business and environment context. |
| `recipes/` | Generated agent and tool recipes. Derived from `Spec.md`. |
| `evals/` | Eval configuration. Derived from `Spec.md`. |
| `gates/` | The four gate definitions and their checks. |
| `.github/agents/` | Six orchestrating agents. |
| `.github/skills/` | Nine skills — the institutional knowledge layer. |
| `.github/commands/` | The human-facing command palette. |
| `examples/golden-thread/` | One worked problem end to end. Start here to learn the system. |

---

## Start here

### Prerequisites

- [ ] The upstream **business blueprint package** is available — process maps, workshop
      outputs, problem and value statements, personas, supporting documents.
- [ ] Enterprise **policy concurrence** for your domain is complete, per whatever
      approval process your organization requires before project execution.
- [ ] A dedicated **cloud resource group** (or equivalent isolated environment) is provisioned.
- [ ] A **project repository** is provisioned.

> Record what these map to in your organization in
> [`project_context/local_context.md`](project_context/local_context.md).
> This repository deliberately stays neutral about them.

### First 30 minutes

| Time | Step | What happens |
|---|---|---|
| 0–5 min | **Bootstrap** | Clone or fork this starter into the provisioned project repository. Open it in your AI-assisted IDE. |
| 5–10 min | **Load source** | Place the supplied blueprint artifacts under `business-blueprint/supplied-source-documents/`. Connect enterprise context sources. |
| 10–20 min | **Draft intent** | Run `/intake-blueprint`, then `/intent-from-blueprint`. The AI synthesizes the package, preserves provenance and lists unresolved gaps. |
| 20–30 min | **Challenge and route** | Run `/roast-intent`. Resolve what the project team can answer; package the remainder for the blueprint team's review and sign-off. |

### The full sequence

1. Confirm the upstream business blueprint package is available.
2. Complete your organization's policy concurrence for the domain.
3. Obtain the dedicated cloud resource group and project repository.
4. Bootstrap this starter into that repository.
5. Place supplied blueprint artifacts under `business-blueprint/`.
6. Run `/intake-blueprint`, then `/intent-from-blueprint`.
7. Run `/roast-intent`. Send `Intent.md` plus unresolved questions to the business blueprint team.
8. **Obtain business sign-off.** Version the approved `Intent.md`.
9. Run `/blueprint` using the approved Intent plus enterprise standards.
10. Run `/roast-spec`. Obtain technical sign-off on `Spec.md`.
11. Run `/package` — reuse/create qualification, then compile recipes and evals.
12. Run `/plan`, then build from the approved baseline.
13. Route material demo feedback through `/reconcile-feedback`.
14. **Re-sign Intent or Spec after material change** before continuing downstream.

Steps 8 and 10 are human sign-offs. No agent in this repository can perform them.

---

## The two rules that make this work

**Authority.** A recipe that contradicts `Spec.md` loses — the Spec wins. A Spec that
contradicts `Intent.md` means either the Intent is amended or the Spec is corrected.
The implementation layer cannot silently redefine scope.

**Volatility.** Moving a button does not rewrite Intent. Adding a persona, a decision
authority or a business outcome probably does.

And one dependency rule worth stating on its own: **an Intent change makes the previous
Spec baseline stale.** A Spec-only change does not automatically reopen Intent.

---

## Tool neutrality

The methodology is defined in terms of **skills**, **agents** and **gates**. The
`.github/` directory is a *distribution adapter*, not the method. The semantics of
Intent, Spec, change reconciliation and gates stay constant across any approved
assistant or IDE. To add another toolchain, write another adapter over the same
`.github/skills/` and `.github/agents/` content.

Adapters included: `.github/copilot-instructions.md` (VS Code / Copilot),
[`AGENTS.md`](AGENTS.md) (cross-tool convention), `.github/commands/` (command palette).

---

## Scope of this edition

**Covered:** design → blueprint → package → plan → prototype → reconcile.

**Deliberately not covered:** automated testing depth, deployment, security release
gates, UAT controls, production readiness. These remain accountable stages in their own
right and are the natural next layer.
