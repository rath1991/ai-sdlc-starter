# /roast-intent

Adversarially challenge the draft Intent before business review.

**Skill:** `intent-roast` · **Gate:** Intent Ready
**Requires:** `Intent.md` drafted

## Steps

Follow [`.github/skills/intent-roast/SKILL.md`](../skills/intent-roast/SKILL.md).

Work the interrogation: why AI · where is the baseline · how is success measured · who
signed up · what would make this fail · is the scope real · are the personas real · is the
evidence real · is the problem the real problem.

Then run the Intent Ready gate
([`gates/intent-ready.md`](../../gates/intent-ready.md)) and report every check.

## Output

Roast report with BLOCKING / MAJOR / MINOR findings, the gate result, questions routed to
the business blueprinting team — **and what is sound.** A roast that lists only problems
loses the reviewer's ability to tell severity apart.

Set `status: challenged`. Fix what the package supports; everything else becomes an open
question.

## Then

If the verdict is READY, package for business sign-off: draft, blocking questions with
what evidence would close each, contradictions with both readings, inferred claims.

**Stop there.** Only the business blueprint authority can set `status: approved`.
