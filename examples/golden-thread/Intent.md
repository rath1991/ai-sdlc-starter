---
artifact: intent
version: 1.3
status: approved
owner: Reliability Engineering Manager
approved_by:
  - Reliability Engineering Manager
  - Business Blueprinting Lead
approved_date: 2026-03-11
sources:
  - workshop-01
  - process-map-v2
  - maintenance-data-profile-01
  - persona-interviews-01
---

# Intent — Equipment Reliability Risk Identification

> **Illustrative example.** Numbers and roles are fictional.

## 1. Problem

**INT-001** — Equipment failures on the monitored asset population are identified after
they occur, not before. Evidence that would have indicated rising risk exists across
maintenance records, sensor history and inspection notes, but reconstructing it takes an
engineer two to three days per asset — so it is done only after a failure, as part of a
post-mortem, rather than before one.

### Evidence

| Claim | Source | Confidence |
|---|---|---|
| 14 unplanned failures per quarter across 200 monitored assets | maintenance-data-profile-01 | Stated |
| Post-failure evidence reconstruction takes 2–3 engineer-days | workshop-01 §3, persona-interviews-01 | Stated |
| Indicating evidence was present before failure in 9 of 12 reviewed cases | workshop-01 §4 | Stated |
| Engineers do not attempt pre-failure reconstruction because of the time cost | persona-interviews-01 | Inferred — three of four interviewees said this; not stated as policy |

## 2. Current process

**INT-002** — Weekly reliability review across the monitored population.

| Step | Actor | Input | Decision / output | Known pain |
|---|---|---|---|---|
| 1 | Reliability engineer | Sensor threshold alarms | Shortlist assets for review | Alarms are threshold-based; slow degradation does not trip them |
| 2 | Reliability engineer | Maintenance history (system of record) | Assess whether the alarm is meaningful | History is free-text; quality varies by site |
| 3 | Reliability engineer | Inspection notes (separate system) | Look for corroborating evidence | Manual cross-reference; usually skipped under time pressure |
| 4 | Reliability engineer + planner | Assessment | Decide whether to intervene before next planned outage | **The decision this project supports** |
| 5 | Planner | Intervention decision | Schedule work | Out of scope |

> **Process finding.** Step 3 is where the problem lives. Step 4 is the decision the
> system supports. Step 5 is explicitly not touched — see §7.

### Workarounds in use

| Workaround | Who | Compensates for | Implies we need |
|---|---|---|---|
| Personal spreadsheet of "assets I don't trust" | Two senior engineers | No systematic risk memory | Persistent, shared risk signal |
| Asking a long-tenured colleague | All engineers | Undocumented failure history | Evidence reconstruction from records |

## 3. Pain points

| ID | Pain | Who | Frequency | Current cost |
|---|---|---|---|---|
| INT-003a | Cross-system evidence review is too slow to do preventively | Reliability engineer | Weekly, skipped | 2–3 days per asset |
| INT-003b | Slow degradation does not trigger threshold alarms | Reliability engineer | Continuous | Missed early signals |
| INT-003c | Risk knowledge is personal, not institutional | Reliability team | Continuous | Lost on staff change |

## 4. Personas

### Reliability engineer — primary

- **Role.** Assesses asset condition; recommends intervention.
- **Current workflow.** Weekly review of alarm shortlist; manual history lookup where time allows.
- **What they need to decide.** Whether an asset needs intervention before the next planned outage.
- **What they do differently.** Reviews a reconstructed evidence chronology instead of assembling one, and reviews assets that no alarm flagged.
- **What would make them abandon it.** Evidence they cannot verify, or a chronology that reads as confident when the underlying records are thin.

### Maintenance planner — secondary

- **Role.** Schedules work.
- **Workflow change.** Receives intervention requests with evidence attached. **The planner's scheduling decision is not automated** — see §7.

## 5. Target outcome

**INT-004** — A reliability engineer can, for any monitored asset, obtain a reconstructed
chronology of the evidence bearing on its condition, assembled from maintenance records,
sensor history and inspection notes, in minutes rather than days — so that step 4 can be
made preventively rather than after failure.

## 6. Value hypothesis

| Element | Value |
|---|---|
| **Baseline** | 14 unplanned failures / quarter across 200 monitored assets (maintenance system of record, trailing 4 quarters, maintenance-data-profile-01) |
| **Target** | ≤ 10 / quarter within 2 quarters of deployment |
| **How measured** | Same maintenance-system query, run quarterly. Failures classified as flagged / not flagged by the system in the preceding 60 days |
| **Measurement owner** | Reliability Engineering Manager |
| **First review** | End of first full quarter post-deployment |

**Leading measure (INT-020a).** Flagged-before-failure rate — directly attributable.
**Lagging measure (INT-020b).** Unplanned failures per quarter — depends on engineers and
planners acting on what is flagged. Confounders: planned-outage frequency, population
changes, seasonal load.

**Why AI?** Evidence is unstructured free text across three systems, of variable quality,
where the relevant signal is a *pattern across records* rather than any single value. A
threshold rule was the prior approach — it is step 1 of the current process, and INT-003b
is its known failure mode. A report cannot reconstruct a chronology from inconsistent
free-text records.

> `intent-roast` finding, resolved: v1.0 stated the target as "improve reliability," which
> had no baseline, no number and no owner, and could not fail. Resolved in v1.1.

## 7. Scope

### In scope

| ID | In scope |
|---|---|
| INT-010 | Reconstruct an evidence chronology for a named asset from maintenance records, sensor history and inspection notes |
| INT-011 | Surface assets with rising risk indicators that threshold alarms do not flag |
| INT-012 | Present the evidence with source attribution so an engineer can verify every element |
| INT-013 | Report confidence, including explicitly reporting when evidence is insufficient |

### Out of scope

| Out of scope | Why | Revisit when |
|---|---|---|
| **Prescribing a maintenance action** | Intervention decisions carry safety and cost accountability that sits with the engineer and planner. The system provides evidence; humans decide. | Not before two quarters of demonstrated evidence quality |
| **Writing to the maintenance system of record** | Read-only by design; the system of record has its own controls | Not in this initiative |
| Scheduling or work-order creation | Planner's decision, step 5 | Not in this initiative |
| Declaring an asset safe to operate | Regulated determination | Never — out of bounds |
| Real-time monitoring | Weekly review cadence is the process; real-time is a different system | If the process cadence changes |
| Unmonitored asset population | No sensor history to reconstruct from | If instrumentation expands |

> The first exclusion is the one that mattered. See [`CR-001.md`](CR-001.md).

## 8. Success measures

| ID | Measure | Baseline | Target | Source of truth |
|---|---|---|---|---|
| INT-020a | Flagged-before-failure rate | 0% (no capability) | ≥ 50% of unplanned failures flagged in preceding 60 days | System log + maintenance records |
| INT-020b | Unplanned failures / quarter | 14 | ≤ 10 | Maintenance system of record |
| INT-020c | Time to assemble evidence for one asset | 2–3 engineer-days | < 30 minutes | Engineer-reported, sampled |
| INT-020d | Chronology elements verifiable to source | n/a | 100% | Eval |
| INT-020e | Weekly adoption | 0 | ≥ 8 of 10 engineers using it in weekly review by month 3 | System log |

## 9. Assumptions

| ID | Assumption | If false, then | Owner | Validation |
|---|---|---|---|---|
| INT-030 | Maintenance free-text records contain parseable dates in ≥ 90% of entries | Chronology cannot be ordered reliably; core capability fails | Data Engineering | **Profiled: 94% across 3 sites. Two sites unprofiled.** |
| INT-031 | Inspection notes are retrievable programmatically | Manual step remains; value drops materially | Systems owner | Confirmed — API exists |
| INT-032 | Engineers will act on flags not corroborated by an alarm | Leading measure improves, lagging does not | Reliability Manager | Unvalidated — **adoption risk** |
| INT-033 | Sensor history retention covers ≥ 12 months | Slow degradation invisible | Platform | Confirmed — 24 months |

> `intent-roast`: INT-030 is the riskiest assumption and is only partly validated.
> Accepted at sign-off on condition that the remaining two sites are profiled before
> build. See §11.

## 10. Open questions

| ID | Question | Routed to | Blocking | Status |
|---|---|---|---|---|
| INT-040 | Who approves intervention — engineer or planner? workshop-01 and process-map-v2 disagree | Business Blueprinting | yes | **Resolved v1.2: engineer recommends, planner schedules. Both cited; process-map-v2 described the intended process** |
| INT-041 | Will engineers act on non-alarm-corroborated flags? | Reliability Manager | no | Open — tracked as INT-032 |
| INT-042 | Do the two unprofiled sites match the 94% date-parseability finding? | Data Engineering | yes | **Condition of sign-off — see §11** |

## 11. Sign-off

| Role | Name | Date | Decision |
|---|---|---|---|
| Business owner | Reliability Engineering Manager | 2026-03-11 | Approved with condition |
| Business blueprint authority | Business Blueprinting Lead | 2026-03-11 | Approved |

**Condition.** INT-042 must be closed before build begins. If date parseability at the
remaining two sites falls below 85%, INT-010 requires reassessment.

### Revision history

| Version | Date | Change | Classification | CR | Re-signed by |
|---|---|---|---|---|---|
| 1.0 | 2026-02-20 | Initial draft from blueprint package | — | — | — |
| 1.1 | 2026-02-27 | Value hypothesis rewritten with baseline, target, owner | roast finding | — | — |
| 1.2 | 2026-03-05 | INT-040 resolved; approval authority clarified | roast finding | — | — |
| 1.3 | 2026-03-11 | Out-of-scope expanded after scope-boundary check | roast finding | — | Both |

> **Dependency reminder.** Any approved change here marks the downstream `Spec.md`
> baseline stale and requires technical reassessment.
