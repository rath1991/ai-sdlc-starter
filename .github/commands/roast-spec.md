# /roast-spec

Adversarially challenge the draft Spec before technical review.

**Skill:** `architecture-roast` · **Gate:** Design Ready
**Requires:** `Spec.md` drafted

## Steps

Follow [`.github/skills/architecture-roast/SKILL.md`](../skills/architecture-roast/SKILL.md).

Work the interrogation: why is this an agent · why this many · does everything trace to
Intent · who owns what · is the state design deliberate · what happens when it fails · are
the data assumptions survivable · are the platform choices approved · do the evals prove
the promises · is this the simplest design that works.

Then run the Design Ready gate
([`gates/design-ready.md`](../../gates/design-ready.md)) and report every check.

## Output

Roast report, gate result, **components recommended for removal**, simplification
opportunities, exceptions requested, and what is sound.

> The most valuable outcome of this command is usually a smaller system.

Act on the findings before handover. Set `status: challenged`.

## Then

Package for technical sign-off. **Only the technical blueprint authority can set
`status: approved`.**
