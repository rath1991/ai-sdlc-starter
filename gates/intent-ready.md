# Gate — Intent Ready

Advances `Intent.md` from draft to business review, and then to approved.

**Owner:** business blueprint authority
**Runs after:** `intent-synthesis` + `intent-roast`
**Blocks:** technical blueprinting

---

## Deterministic checks

Machine-checkable. Any failure blocks.

| # | Check | Failure |
|---|---|---|
| D1 | `Intent.md` exists with valid frontmatter | block |
| D2 | `artifact: intent` present | block |
| D3 | `version` present and well-formed | block |
| D4 | `owner` populated | block |
| D5 | `sources` non-empty | block |
| D6 | §7 **In scope** has at least one entry with an ID | block |
| D7 | §7 **Out of scope** has at least one entry | block |
| D8 | §6 baseline field is non-empty | block |
| D9 | §6 target field is non-empty | block |
| D10 | §6 measurement owner is named | block |
| D11 | §8 has at least one measure with baseline and target | block |
| D12 | Every §1 claim cites a source ID present in `sources` | block |
| D13 | No §10 question marked `blocking: yes` with `status: open` | block |
| D14 | For `status: approved`: §11 populated with role, name, date | block |

D7 deserves its reputation. An empty out-of-scope section is the reason demo feedback
becomes unanswerable, and it is trivially checkable, so it is checked.

D8–D10 exist because a value claim without a baseline, a target and a named measurer
cannot be proven after deployment, and the project will be asked to prove it.

## Semantic checks

Require a reviewing agent or human. A finding may be accepted with documented reasoning.

| # | Check | Finding if failed |
|---|---|---|
| S1 | Problem is an operational reality, not a missing technology | blocking |
| S2 | Personas are real, appear in sources, and their workflow change is concrete | blocking if inferred and unlabelled |
| S3 | Current process is mapped to the level where the problem is locatable | major |
| S4 | The decision the system supports is identifiable | blocking |
| S5 | Value hypothesis can *fail* — a failing outcome is definable | blocking |
| S6 | Value is attributable to this system, or confounders are named | major |
| S7 | Baseline measurement is reproducible after deployment | blocking |
| S8 | "Why AI?" has a real answer; the deterministic alternative was considered | blocking |
| S9 | Out-of-scope contains what people will actually ask for in the demo | major |
| S10 | Every in-scope item traces to a stated pain point | major |
| S11 | §9 assumptions include a credible failure mode | major |
| S12 | Source contradictions are recorded, not silently resolved | blocking |
| S13 | `Inferred` claims are labelled as inferred | blocking |

## Report

```markdown
## Intent Ready — <project>, Intent v<x.y>

**Deterministic:** n/14 pass
**Semantic:** n blocking, n major, n minor
**Verdict:** BLOCKED | READY FOR BUSINESS REVIEW | APPROVED

### Failed deterministic
| # | Check | Observed | To resolve |

### Semantic findings
| # | Severity | Finding | Recommendation |

### Accepted with reasoning
| # | Finding | Why accepted | Accepted by |

### Open questions routed to business blueprinting
| ID | Question | Blocking |
```

## Advancing

| To | Requires |
|---|---|
| `challenged` | `intent-roast` has run and findings are recorded |
| `business-review` | all deterministic pass; no blocking semantic findings; no open blocking questions |
| `approved` | **human sign-off in §11 by the business blueprint authority** |

> No agent may set `status: approved`.
