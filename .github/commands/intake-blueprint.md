# /intake-blueprint

Validate and index the supplied business blueprint package.

**Run first**, before any Intent work.

## Steps

1. Read everything under `business-blueprint/supplied-source-documents/`.
2. Build `project_context/source_map.yaml` — one entry per document with a stable ID,
   type, date, author, what it covers, and an authority rating.
3. Rate authority honestly: `high` for approved and attributed material, `unclear` for
   anything undated, unattributed or marked draft. Do not treat `unclear` material as
   settled.
4. Report coverage against the Intent sections: which are supported, which are thin,
   which are absent.
5. Flag contradictions between documents. Do not resolve them.

## Report

```markdown
## Blueprint intake

**Documents:** n indexed
**Authority:** n high, n medium, n low, n unclear

### Coverage
| Intent section | Coverage | Sources |
|---|---|---|
| Problem | supported / thin / absent | |

### Contradictions found
| Topic | Reading A (source) | Reading B (source) |

### Absent for Intent drafting
### Documents of unclear authority
```

Stop here. Do not draft Intent — that is `/intent-from-blueprint`. If the package is too
thin to draft from at all, say so plainly rather than proceeding.
