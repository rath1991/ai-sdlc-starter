# Command palette

The human-facing surface. **A user should remember workflows, not internal skill topology.**

Eight commands cover the lifecycle. Each one dispatches to the right agent and skills, so
nobody has to know that `/roast-intent` invokes the `intent-roast` skill, or that
`/package` runs two agents in sequence.

| Command | Does | Agent | Produces |
|---|---|---|---|
| `/intake-blueprint` | Validate and index the supplied package | — | `source_map.yaml` |
| `/intent-from-blueprint` | Draft a provenance-backed Intent | `intent-synthesis` | `Intent.md` |
| `/roast-intent` | Challenge the business case | `intent-synthesis` | Roast report |
| `/blueprint` | Create the technical Spec | `technical-blueprint` | `Spec.md` |
| `/roast-spec` | Challenge the architecture | `technical-blueprint` | Roast report |
| `/package` | Reuse/create + compile recipes | `reusable-qualification`, `recipe-compiler` | `recipes/`, `evals/`, `traceability.yaml` |
| `/plan` | Generate the engineering plan | `planning` | `plan.md` |
| `/reconcile-feedback` | Classify demo feedback | `change-reconciliation` | `changes/CR-*.md` |

## Normal order

```
/intake-blueprint → /intent-from-blueprint → /roast-intent
   → [BUSINESS SIGN-OFF]
      → /blueprint → /roast-spec
         → [TECHNICAL SIGN-OFF]
            → /package → /plan → build
               → /reconcile-feedback  (as feedback arrives)
```

The two bracketed steps are human sign-offs. No command performs them.

## Portability

These are plain Markdown. Adapt the invocation syntax to your assistant; the content is
the instruction either way.
