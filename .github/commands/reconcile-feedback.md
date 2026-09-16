# /reconcile-feedback

Classify stakeholder feedback and route it to the owning team.

**Agent:** `change-reconciliation` · **Gate:** Change Control

## Run this when

- Demo or review feedback arrives.
- An eval failure turns out to be a Spec defect rather than a bug.
- Anyone proposes something that might change what the system means.

## Steps

Follow [`.github/agents/change-reconciliation.agent.md`](../agents/change-reconciliation.agent.md).

1. **Capture verbatim.** The stakeholder's actual words in CR §1, plus who and in what
   context. Raw request is evidence; your reading is analysis. Different sections.
2. **Classify** — work the tests in order, stop at the first that fires:

   | | Test | → |
   |---|---|---|
   | 1 | Does any Intent §7 scope item change? | **C** |
   | 2 | New persona, decision authority or business outcome? | **C** |
   | 3 | Behavior, architecture, data or platform change? | **B** |
   | 4 | Only presentation or local implementation? | **A** |
   | 5 | Valuable but not now? | **D** |

3. **Resolve ambiguity upward.** A/B → B. B/C → C. An unnecessary reassessment costs a
   meeting; a missed one costs an unapproved change in a signed artifact.
4. **Check cumulative drift.** Five correctly classified A changes can compose into a
   behavioral change nobody approved. Raise the trend with the CRs listed.
5. **Walk `traceability.yaml`** for affected IDs and downstream staleness.
6. **State the full cascade** for C, so the decision-maker sees the real cost.
7. **Route and notify.** Set `status: routed`. **Then stop.**

## The boundary

You classify, identify ownership and notify. **You do not approve.**

CR §5–7 require a named human — always, including when the change is obviously fine and
everyone agrees. An agent that approves its own scope expansions is not a governance
system.

## After the human decides

Update `traceability.yaml` `changes:`, trigger regeneration, set `status: implemented`,
update the index in `changes/README.md`. For a deferral, ensure the revisit trigger is
somewhere someone will actually see.
