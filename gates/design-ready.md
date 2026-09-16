# Gate — Design Ready

Advances `Spec.md` from draft to technical review, and then to approved.

**Owner:** technical blueprint authority
**Runs after:** `technical-blueprint` + `architecture-roast`
**Blocks:** packaging and build

---

## Deterministic checks

| # | Check | Failure |
|---|---|---|
| D1 | `Intent.md` is `status: approved` | block |
| D2 | `Spec.md` exists with valid frontmatter | block |
| D3 | `derived_from.intent` == approved `Intent.md` version | block — **stale baseline** |
| D4 | `owner` populated | block |
| D5 | Every in-scope `INT-*` appears in §1, addressed or deliberately deferred | block |
| D6 | Every `SPEC-*` in §3 carries an Intent ID — **no orphans** | block |
| D7 | Every Intent ID referenced exists in `Intent.md` | block |
| D8 | Every agent in §4 has a §3 justification entry | block |
| D9 | Every agent has a stated autonomy boundary | block |
| D10 | Every component in §3 appears in §8 failure behavior | block |
| D11 | §8 degradation policy is non-empty | block |
| D12 | §6 data quality assumptions non-empty, each with violation handling | block |
| D13 | §10 reuse/create decision present for every capability | block |
| D14 | Every §11 eval declares what it proves | block |
| D15 | Every eval's `proves` IDs exist | block |
| D16 | §11 out-of-contract table non-empty | block |
| D17 | Every platform service in §7 is in the approved set, or has an exception with a named approver | block |
| D18 | For `status: approved`: §12 populated | block |

D3 is the check that makes this system work over time. If the business team amended Intent
to 1.4 and the Spec still says `derived_from.intent: 1.3`, the design implements a
superseded business baseline — and every recipe compiled from it inherits that.

D6 is the orphan check. A component tracing to no requirement means either a requirement is
missing or the component should not exist. Both need a decision.

## Semantic checks

| # | Check | Finding if failed |
|---|---|---|
| S1 | Every agent passes the reasoning test — must be *permitted* to differ, by judgment | blocking |
| S2 | No component performs a deterministic transformation as an agent | blocking |
| S3 | Agent count reflects the problem, not the pattern | major |
| S4 | No orchestrator exists solely to call others in fixed order | major |
| S5 | §2 behavior is comprehensible without the §3 diagram | major |
| S6 | Nothing is designed for Intent §7 out-of-scope behavior | blocking |
| S7 | State is stateless by default; persistence is justified with retention and staleness handling | major |
| S8 | Ownership is clear per component; no unagreed owners | major |
| S9 | No two components write the same state | major |
| S10 | The system never degrades into confident wrong answers | blocking |
| S11 | "Return nothing, with a reason" is available | blocking |
| S12 | Tool errors are surfaced, never substituted | blocking |
| S13 | Data assumptions were checked against actual data, not just the schema | major |
| S14 | Evals could not all pass while the system is useless | major |
| S15 | Every Intent §7 exclusion is represented as a negative eval | major |
| S16 | Reuse decisions are honest — no fork recorded as REUSE | blocking |
| S17 | Not an all-CREATE capability map | major |

## Report

```markdown
## Design Ready — <project>, Spec v<x.y> (Intent v<x.y>)

**Deterministic:** n/18 pass
**Semantic:** n blocking, n major, n minor
**Verdict:** BLOCKED | READY FOR TECHNICAL REVIEW | APPROVED

### Failed deterministic
### Semantic findings
### Components recommended for removal
### Exceptions requested
| Constraint | Requested | Approver | Status |
### Accepted with reasoning
```

## Advancing

| To | Requires |
|---|---|
| `challenged` | `architecture-roast` has run and findings are recorded |
| `technical-review` | all deterministic pass; no blocking semantic findings |
| `approved` | **human sign-off in §12 by the technical blueprint authority** |

> No agent may set `status: approved`.
