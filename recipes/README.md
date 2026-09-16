# Recipes

**Compiled output.** The Spec is the source.

| | |
|---|---|
| `agents/` | One recipe per agent component in `Spec.md` §4 |
| `tools/` | One contract per tool in `Spec.md` §5 |
| `schemas/` | JSON schemas referenced by recipes |

## The rule

**Where a recipe contradicts the Spec, the Spec wins.**

If compiling reveals a Spec defect — missing contract, ambiguous behavior, impossible
constraint — report it. Do not resolve it here.

A recipe that silently diverges from its Spec is the most corrosive artifact in this
system, because everything still *looks* correct: the IDs match, traceability validates,
and the built system does something the approved design never said. Nothing downstream
catches it.

## Two fields to get right

**`autonomy.must_not`** — where `Intent.md` §7 out-of-scope becomes machine-readable. Copy
each exclusion with its Intent reference. This makes scope drift visible in code review: a
pull request deleting a `must_not` line reads as what it is.

**`failure`** — explicit for every case in `Spec.md` §8. A recipe with no declared failure
behavior produces a component that invents its own, usually by returning something
confident.

## Reuse

A capability marked REUSE in `Spec.md` §10 is **consumed and configured**. Copying the
asset here and editing it is CREATE — and the worse form of it, since you inherit the
maintenance burden and lose upstream fixes.

Format and full standard: [`.github/skills/recipe-generation/SKILL.md`](../.github/skills/recipe-generation/SKILL.md).
