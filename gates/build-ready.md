# Gate — Build Ready

Advances an approved Spec to a compiled, buildable package.

**Owner:** delivery, on compiler validation
**Runs after:** `reusable-qualification` + `recipe-compiler`
**Blocks:** `planning` and build

---

This gate is **almost entirely deterministic**. By this point the semantic judgments have
been made and signed; what remains is verifying that the compiled output faithfully
represents the approved design.

## Deterministic checks

| # | Check | Failure |
|---|---|---|
| D1 | `Intent.md` is `status: approved` | block |
| D2 | `Spec.md` is `status: approved` | block |
| D3 | `Spec.derived_from.intent` == approved Intent version | block — **stale** |
| D4 | §10 reuse/create complete for every capability | block |
| D5 | One recipe exists per agent component in §4 | block |
| D6 | Every recipe declares `spec_id` and `intent_ids` | block |
| D7 | Every recipe `spec_id` exists in `Spec.md` | block — **divergence** |
| D8 | Every recipe `intent_ids` exists in `Intent.md` | block |
| D9 | Every tool referenced has a contract file that resolves | block |
| D10 | Every tool contract declares idempotency | block |
| D11 | Every tool contract declares typed errors with retryability | block |
| D12 | Every recipe declares `failure` for each Spec §8 case | block |
| D13 | Every recipe with an Intent §7 exclusion declares it in `autonomy.must_not` | block |
| D14 | `evals/eval_config.yaml` exists | block |
| D15 | Every Spec §11 eval appears in the config | block |
| D16 | Every eval declares `proves`, with existing IDs | block |
| D17 | Every out-of-contract behavior has an `out_of_contract` eval | block |
| D18 | Every eval references a dataset path that resolves | block |
| D19 | `traceability.yaml` exists and parses | block |
| D20 | No `SPEC-*` in traceability without an `INT-*` | block |
| D21 | Every in-scope `INT-*` appears in a chain | block |
| D22 | Every recipe path in traceability resolves | block |
| D23 | No unknown IDs anywhere | block |

D7 and D23 are the divergence checks, and they fail loudly by design. An unknown SPEC ID
means the recipe and the Spec no longer describe the same system. Report the specific ID —
`recipe failure_chronology.yaml references SPEC-019, absent from Spec.md v2.1` — not a
generic validation failure.

D13 is what makes scope boundaries survive compilation. If Intent excludes prescriptive
recommendations and no recipe records that exclusion, the boundary exists only in a
document nobody reads at build time.

## Semantic checks

| # | Check | Finding if failed |
|---|---|---|
| S1 | No recipe contradicts the Spec | blocking |
| S2 | No Spec defect was resolved silently in a recipe | blocking |
| S3 | REUSE recipes consume the asset rather than vendoring and editing it | blocking |
| S4 | Recipe failure behavior matches Spec §8 in substance, not just field presence | major |
| S5 | Eval pass criteria are meaningful, not trivially satisfiable | major |
| S6 | Eval datasets actually exercise the stated behavior | major |

## Report

```markdown
## Build Ready — <project>, Spec v<x.y>

**Deterministic:** n/23 pass
**Verdict:** BLOCKED | READY TO BUILD

### Failed deterministic
| # | Check | Specific ID / path | To resolve |

### Spec defects reported (not resolved)
| Defect | Spec § | Reported to |

### Compilation summary
| Recipes | Tool contracts | Evals | Out-of-contract evals |
```

## Advancing

All deterministic checks pass and no blocking semantic findings → `planning` may proceed.

Unlike Intent Ready and Design Ready, this gate needs no separate human sign-off — the
governing artifacts were already signed. What this gate asserts is that the compiled
package is a faithful representation of them.
