---
artifact: spec
version: 2.1
status: approved
owner: Solution Architect
derived_from:
  intent: 1.3
approved_by:
  - Solution Architect
  - Technical Blueprint Authority
approved_date: 2026-03-24
---

# Spec — Equipment Reliability Risk Identification

> **Illustrative example.** Abbreviated to show structure and the decisions that matter.
> Designed against approved `Intent.md` v1.3.

## 1. Intent coverage

| Intent ID | In-scope item | Addressed by | Notes |
|---|---|---|---|
| INT-010 | Reconstruct evidence chronology | SPEC-011 | Core novel capability |
| INT-011 | Surface rising risk not flagged by alarms | SPEC-014 | Runs across population weekly |
| INT-012 | Source attribution for every element | SPEC-011, SPEC-012 | Attribution carried through retrieval |
| INT-013 | Report confidence, including insufficiency | SPEC-011 | `confidence` enum includes `insufficient` |

### Deliberately not addressed

| Intent ID | Why not in this release |
|---|---|
| INT-020e (adoption) | Measured, not built. Adoption depends on process change, tracked by the Reliability Manager. |

## 2. Solution behavior

**SPEC-001** — Two entry points.

**On demand.** An engineer names an asset. The system retrieves maintenance records,
sensor history and inspection notes for the window, normalizes timestamps, orders events
into a chronology, attributes each element to its source, and reports a confidence level.
Where records are too thin to order reliably, it returns `insufficient` and says why rather
than producing a chronology.

**Weekly sweep.** Across the monitored population, the system scores rising-risk indicators
not captured by threshold alarms, and returns a ranked shortlist with the evidence for each.

| # | Trigger | Behavior | Output | Intent ID |
|---|---|---|---|---|
| 1 | Engineer requests asset | Retrieve → normalize → order → attribute → score confidence | Chronology + confidence | INT-010, INT-012, INT-013 |
| 2 | Weekly schedule | Score population, rank | Shortlist + evidence | INT-011 |

## 3. Architecture

```
                    ┌──────────────────────────┐
   engineer ───────▶│ SPEC-011                 │
                    │ FailureChronologyAgent   │◀── SPEC-012 document_retrieval (tool)
                    │  (agent — judgment)      │◀── SPEC-013 timestamp_normalize (tool)
                    └──────────────────────────┘◀── SPEC-015 sensor_history_query (tool)
                                 │
   schedule ──▶ SPEC-014 ────────┘
               RiskIndicatorSweep
               (scheduled job — not an agent)
```

| ID | Component | Type | Responsibility | Intent ID |
|---|---|---|---|---|
| SPEC-011 | FailureChronologyAgent | agent | Order and interpret evidence under conflict and ambiguity | INT-010, INT-012, INT-013 |
| SPEC-012 | document_retrieval | tool | Retrieve records within scope | INT-010, INT-012 |
| SPEC-013 | timestamp_normalize | tool | Normalize varied date formats | INT-010 |
| SPEC-014 | RiskIndicatorSweep | scheduled job | Score population, rank, invoke SPEC-011 for top N | INT-011 |
| SPEC-015 | sensor_history_query | tool | Query sensor time series | INT-010 |

### Why each agent is an agent

| Component | Why it must reason rather than execute | Alternative considered |
|---|---|---|
| SPEC-011 FailureChronologyAgent | Records conflict, overlap and are inconsistently worded. Deciding which of two contradictory entries is authoritative, whether thin evidence supports a chronology at all, and how to characterize confidence are judgments — two runs on genuinely ambiguous input may legitimately order events differently, and the design must permit that while reporting the ambiguity. | Rule-based ordering: fails on conflicting timestamps and cannot assess sufficiency |

> **`architecture-roast` finding, acted on.** Spec v1.0 proposed `NormalizationAgent`,
> justified as "handles varied timestamp formats." That is parsing, not judgment — the
> reasoning test failed. Demoted to SPEC-013, a tool with unit tests. Removing it took one
> source of non-determinism, one LLM call per record, and roughly 40% of per-request
> latency out of the design.
>
> **Second finding, acted on.** v1.0 proposed `SweepOrchestratorAgent` to run the weekly
> sweep. It called two components in fixed order and decided nothing. Now SPEC-014, a
> scheduled job.

## 4. Agents

### FailureChronologyAgent — SPEC-011

- **Purpose.** Reconstruct and interpret the evidence chronology for one asset.
- **Invoked by.** Engineer on demand; SPEC-014 for shortlisted assets.
- **Skills.** `evidence-ordering`
- **Tools.** SPEC-012, SPEC-013, SPEC-015
- **Inputs.** `equipment_id` (required), `window` (default 90 days)
- **Outputs.** `chronology` (schema), `confidence` ∈ {high, medium, low, insufficient}
- **State.** **Stateless.** Each invocation retrieves fresh. No memory — a cached
  chronology could be stale against a record added since, and would be served with
  confidence.
- **Autonomy boundary.**
  - Must not emit a prescriptive maintenance recommendation *(Intent §7)*
  - Must not write to the maintenance system of record *(Intent §7)*
  - Must not declare an asset safe to operate *(Intent §7)*
- **Failure behavior.** See §8.

## 5. Tools

### document_retrieval — SPEC-012

- **Contract.** `(query: string, scope: object, limit: int ≤ 100) → RetrievalResult`
- **Idempotency.** Idempotent.
- **Side effects.** Read-only.
- **Errors.** `INVALID_PARAMETER`, `NOT_FOUND`, `UNAUTHORIZED`, `UPSTREAM_UNAVAILABLE`,
  `TIMEOUT`. **Never returns empty in place of an error** — an empty result means no
  records matched, and the agent must be able to trust that distinction.
- **Authorization.** User identity, not service identity. Site-level record access varies
  by engineer and must be preserved.

### timestamp_normalize — SPEC-013

- **Contract.** `(raw: string, site_id: string) → {timestamp: iso8601, confidence: enum} | AMBIGUOUS`
- **Idempotency.** Pure function.
- **Errors.** `AMBIGUOUS` when a date could be read two ways — returned explicitly rather
  than guessed, so the agent can report the conflict rather than inherit it silently.

## 6. Data

| Dataset | Source | Owner | Classification | Refresh | Quality assumption |
|---|---|---|---|---|---|
| Maintenance records | System of record | Maintenance Ops | Internal | Near-real-time | Free text; ≥ 90% parseable dates |
| Sensor history | Historian | Platform | Internal | 5-min intervals | 24-month retention |
| Inspection notes | Inspection system | Inspection team | Internal | On completion | Semi-structured; API available |

### Data quality dependencies

| Assumption | If violated | Detection | Mitigation |
|---|---|---|---|
| ≥ 90% of maintenance entries have parseable dates *(INT-030)* | Chronology unorderable; core capability fails | SPEC-013 `AMBIGUOUS` rate per site, monitored | Below 85% at a site → return `insufficient` for that site rather than an unreliable chronology |
| Sensor retention ≥ 12 months *(INT-033)* | Slow degradation invisible | Retention check at query time | Report window actually covered |
| Inspection notes retrievable *(INT-031)* | Evidence incomplete | API health | Proceed with reduced confidence, state what is missing |

> Profiled at 94% across three sites. **Two sites unprofiled — Intent §11 sign-off
> condition INT-042.** Build must not begin for those sites until closed.

## 7. Enterprise constraints

| Constraint | Source standard | Compliance |
|---|---|---|
| Approved platform services only | *(see `project_context/local_context.md`)* | All components on approved services |
| User identity for data access | Identity standard | SPEC-012 uses user identity; site permissions preserved |
| Read-only to systems of record | Data standard | No write path exists in the design |
| Responsible AI — human decision authority | RAI standard | Autonomy boundary; prescription excluded |
| Data residency | Data standard | Processing in-region |

## 8. Failure behavior

| Component | Failure mode | Detection | Response | User sees |
|---|---|---|---|---|
| SPEC-011 | Insufficient evidence | Record count / parseability below threshold | Return empty chronology, `confidence: insufficient` | "Insufficient records to reconstruct a reliable chronology" + what was found |
| SPEC-011 | Contradictory timestamps | SPEC-013 `AMBIGUOUS` | **Report the conflict in the output** | Both readings, flagged |
| SPEC-011 | Tool error | Typed error | **Surface — never substitute** | "Could not retrieve from <source>" |
| SPEC-012 | Upstream unavailable | `UPSTREAM_UNAVAILABLE` | Retry with backoff, then surface | Named unavailable source |
| SPEC-014 | Partial population failure | Per-asset error | Complete the rest; report coverage | Shortlist + "n assets could not be assessed" |

### Degradation policy

**The system returns nothing rather than something unreliable.** When evidence is
insufficient, a source is unavailable, or parseability is below threshold, it reports that
state explicitly and names what is missing.

It never produces a chronology it cannot attribute, and never expresses confidence the
evidence does not support. For a system whose output feeds an intervention decision with
safety and cost consequences, a confident wrong chronology is worse than no chronology —
and worse than the status quo, because the engineer has no reason to distrust it.

## 9. Observability

| Signal | Type | Emitter | Consumer | Threshold |
|---|---|---|---|---|
| `chronology_requested` | metric | SPEC-011 | Adoption (INT-020e) | — |
| `confidence_level` | metric | SPEC-011 | Quality | `insufficient` > 30% → investigate |
| `ambiguous_rate` | metric | SPEC-013 | **Data quality (INT-030)** | > 15% per site → alert |
| `flagged_before_failure` | metric | SPEC-014 | **Value (INT-020a)** | Quarterly review |
| `tool_error` | log | all tools | Ops | Any `UNAUTHORIZED` → alert |

## 10. Reuse and create decisions

| Capability | Decision | Existing asset | Justification | Adaptation |
|---|---|---|---|---|
| Document retrieval | **REUSE** | `catalogue://retrieval/document-retrieval@2.3.0` | Covers scoped retrieval with attribution | Configuration + identity passthrough |
| Timestamp normalization | **EXTEND** | `catalogue://data/date-parse@1.4.0` | Covers common formats; site-specific conventions are additive | Site format table — **contributed upstream** |
| Sensor time-series query | **REUSE** | `catalogue://historian/ts-query@1.1.0` | Direct fit | Configuration |
| Evidence ordering under conflict | **CREATE** | *(searched: ordering, chronology, timeline, sequence — no match)* | No approved asset performs conflict-aware ordering with sufficiency assessment | — |
| Population risk scoring | **CREATE** | *(searched: scoring, ranking, anomaly — partial matches unsuitable)* | Existing anomaly detection is threshold-based; INT-003b is its known failure mode | — |

> Two CREATEs of five capabilities. Evidence ordering was flagged to the platform team as
> a candidate for the shared catalogue — it is likely generic across asset-intensive
> domains.

## 11. Eval criteria

| Eval | Proves | Measured | Pass criterion | Data |
|---|---|---|---|---|
| EVAL-007 | INT-004, SPEC-011 | Ordering under conflicting timestamps | Matches ground truth ≥ 95% | `chronology_conflicts.jsonl` |
| EVAL-008 | INT-013, SPEC-011 | Insufficiency detection | Returns `insufficient` on ≥ 98% of thin-evidence cases; **zero false-confident** | `thin_evidence.jsonl` |
| EVAL-009 | INT-012, SPEC-011 | Source attribution | 100% of elements attributable | `attribution.jsonl` |
| EVAL-010 | INT-011, SPEC-014 | Detection of non-alarm risk | Flags ≥ 70% of known cases | `historical_failures.jsonl` |
| EVAL-011 | INT-030, SPEC-013 | Parseability | `AMBIGUOUS` rate < 10% on real records | `date_formats.jsonl` |

### Out-of-contract behavior

| Must not | Eval | Detection |
|---|---|---|
| Emit a prescriptive maintenance recommendation *(Intent §7)* | **EVAL-012** | Zero prescriptive outputs across `prescription_bait.jsonl` — inputs written to invite one |
| Express high confidence on thin evidence | EVAL-008 | Zero false-confident cases |
| Declare an asset safe to operate *(Intent §7)* | **EVAL-013** | Zero safety determinations |
| Write to the maintenance system of record *(Intent §7)* | EVAL-014 | No write path exists; verified by contract review |

> EVAL-012 is the eval that caught CR-001 in regression. See [`CR-001.md`](CR-001.md).

## 12. Sign-off

| Role | Name | Date | Decision |
|---|---|---|---|
| Solution architect | Solution Architect | 2026-03-24 | Approved |
| Technical blueprint authority | Technical Blueprint Authority | 2026-03-24 | Approved |

### Revision history

| Version | Date | Intent v | Change | CR | Re-signed |
|---|---|---|---|---|---|
| 1.0 | 2026-03-14 | 1.3 | Initial draft | — | — |
| 2.0 | 2026-03-19 | 1.3 | `NormalizationAgent` → tool; `SweepOrchestratorAgent` → scheduled job | roast | — |
| 2.1 | 2026-03-24 | 1.3 | Degradation policy; EVAL-012/013 added | roast | Both |

> **Staleness check.** `derived_from.intent: 1.3` matches approved Intent v1.3. Current.
