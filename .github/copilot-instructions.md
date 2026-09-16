# Workspace instructions

This is a **governed AI-SDLC delivery workspace**. Business intent and solution design are
versioned, signed artifacts — not documentation you may freely edit.

> Full instructions: [`AGENTS.md`](../AGENTS.md). Operating model:
> [`PLAYBOOK.md`](../PLAYBOOK.md). This file is the workspace adapter; the methodology is
> tool-neutral.

## Authority

```
Intent.md → Spec.md → recipes/ + evals/ → code
```

Recipe contradicts Spec → **Spec wins**. Spec contradicts Intent → amend Intent through the
change process, or correct the Spec. Never resolve a conflict by editing upward.

## Three hard rules

**You cannot approve.** Never set `status: approved`, never fill `Intent.md` §11 or
`Spec.md` §12, never complete `changes/CR-*.md` §5–7. Those need a named human.

**Check status before generating.** `Intent.md` must be `approved` before Spec work.
`Spec.md` must be `approved` before recipes or planning. If `Spec.md`
`derived_from.intent` is behind the approved Intent version, the Spec is **stale** — stop.

**Material change goes through `/reconcile-feedback`.** If a task requires changing what
the system *means*, do not edit the governing artifact to fit the implementation.

## Assets in this workspace

| Where | What |
|---|---|
| [`.github/agents/`](agents/) | Six agents — one per lifecycle transformation |
| [`.github/skills/`](skills/) | Nine skills — institutional knowledge. Invoke; do not re-derive |
| [`.github/commands/`](commands/) | Eight commands — the human-facing surface |
| [`../gates/`](../gates/) | Four gates — deterministic and semantic checks |

## Editing rules

| Path | |
|---|---|
| `business-blueprint/` | **Read-only** source evidence |
| `Intent.md`, `Spec.md` | Canonical. Only via the owning agent or an approved CR |
| `plan.md` | Free to change |
| `recipes/`, `evals/`, `traceability.yaml` | Compiled. Regenerate; never hand-patch to pass a check |
| `project_context/` | Safe to update |

## Frequent mistakes

- Inventing a baseline number instead of recording it absent and blocking.
- Resolving a contradiction between source documents instead of reporting it.
- Making something an agent when identical input need not produce different output — that
  is a tool.
- A tool returning empty instead of an error, so the agent concludes there is no evidence.
- Patching `traceability.yaml` to make validation pass, when the real finding is that
  recipes and Spec have diverged.
- Reporting a step complete with failing evals.

## Conventions

`INT-*` · `SPEC-*` · `EVAL-*` · `CR-*`. Every `SPEC-*` traces to an `INT-*`. Every eval
declares what it proves. Intent §7 exclusions become `autonomy.must_not` in recipes and
`out_of_contract` evals.
