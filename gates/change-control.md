# Gate — Change Control

Governs feedback becoming approved change.

**Owner:** the team owning the affected artifact
**Runs after:** `change-reconciliation`
**Blocks:** downstream regeneration after a material change

---

## The principle

Feedback is welcome. Silent scope drift is not.

Every meaningful demo comment is classified before it becomes work. Only changes that alter
the business or solution contract propagate upward; cosmetic implementation refinement stays
local.

## Deterministic checks

| # | Check | Failure |
|---|---|---|
| D1 | A `changes/CR-*.md` exists for every classification B, C or D change | block |
| D2 | CR §1 records the feedback verbatim, with who and in what context | block |
| D3 | `classification` is one of A, B, C, D | block |
| D4 | CR §2 rationale is non-empty | block |
| D5 | CR §3 names affected `INT-*` / `SPEC-*` IDs, and they exist | block |
| D6 | For B and C: owning team named in §4 | block |
| D7 | For B and C: notification date recorded | block |
| D8 | CR §5–7 are completed by a **named human**, not an agent | block |
| D9 | For C: `Intent.md` version bumped and §11 re-signed before regeneration | block |
| D10 | For C: `Spec.md` marked for reassessment | block |
| D11 | For B: `Spec.md` version bumped and §12 re-signed before regeneration | block |
| D12 | After re-sign: `Spec.derived_from.intent` matches the new Intent version | block |
| D13 | `traceability.yaml` `changes:` updated with classification, affects, regenerate | block |
| D14 | For deferred (D): revisit condition and owner recorded | block |
| D15 | `changes/README.md` index updated | block |

D8 is the one that makes this a governance system rather than a logging system. An agent
may draft sections 1–4. Sections 5–7 require a named human, every time, including when the
change is obviously fine.

## Semantic checks

| # | Check | Finding if failed |
|---|---|---|
| S1 | The classification is correct under the reference tests | blocking |
| S2 | Ambiguity resolved **upward** — A/B → B, B/C → C | blocking |
| S3 | Downstream impact is complete; nothing stale is left unlisted | blocking |
| S4 | For C: the full cascade is stated, so the decision-maker sees the real cost | major |
| S5 | **No cumulative drift** — successive A changes have not added up to an unapproved behavioral change | major |
| S6 | The change does not contradict an enterprise constraint | blocking |
| S7 | A deferral has a revisit trigger someone will actually see | major |

S5 is the check that survives a correctly applied process. Five classification-A
refinements, each correctly classified, can compose into a behavioral change nobody
approved. Look at the trend, not just the individual CR, and raise it with the CRs listed.

## The cascade for classification C

```
Intent reassessment → business re-sign → Intent version bump
  → Spec becomes stale (derived_from.intent now behind)
    → Spec reassessment → technical re-sign
      → recipes, evals, traceability regenerated
```

State this in the CR. A stakeholder deciding whether to accept a change should see that it
costs two sign-offs and a regeneration — not just a sentence of agreement.

## Report

```markdown
## Change Control — CR-<nnn>

**Classification:** A | B | C | D
**Deterministic:** n/15 pass
**Verdict:** BLOCKED | ROUTED | APPROVED | DEFERRED | REJECTED

### Affected
| Artifact | IDs | Change required |

### Cascade required
### Failed deterministic
### Semantic findings
### Cumulative drift assessment
<!-- Reviewed CRs since the last baseline. Any trend? -->
```

## What no agent may do

- Approve a change
- Set any artifact to `status: approved`
- Complete CR §5–7
- Implement a B or C change before re-sign
- Decide a change is "small enough" to skip the record

> `change-reconciliation` classifies, identifies ownership and notifies. **It routes. It
> does not approve.**
