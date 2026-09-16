# /plan

Generate the engineering plan and drive the build loop.

**Agent:** `planning` · **Requires:** Build Ready passed

## Steps

Follow [`.github/agents/planning.agent.md`](../agents/planning.agent.md).

1. Verify version consistency. A stale Spec version in the plan is a gate failure.
2. **Sequence so the riskiest assumption is tested first** — not by dependency order.
   Every step must produce something evaluable.
3. Map components to files, respecting the reuse decisions. A REUSE capability is consumed
   and configured, never vendored and edited.
4. Dependencies, risks — each risk with an **escalation trigger**.
5. Attach evals to steps.

## The build loop

```
plan → build → evaluate → diagnose → modify
```

**Modify the smallest affected component while preserving the governing Spec.**

When an eval fails, diagnose which of three cases you are in:

| Diagnosis | Action |
|---|---|
| Implementation is wrong | Fix it. Local. |
| Plan is wrong | Change the plan. Local. |
| **Spec is wrong** | **Stop. Run `/reconcile-feedback`.** |

You are in the third case if the fix requires changing what the system *means*, requires
behavior Intent §7 excludes, requires a data source the Spec does not name, or has you
editing `autonomy.must_not`.

Quietly adjusting behavior until the eval passes converts a design defect into
undocumented scope drift.

## Reporting

Never report a step complete with failing evals. Say which fail, what you diagnosed, what
you recommend. Report skipped steps as skipped. Log every iteration.
