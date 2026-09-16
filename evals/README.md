# Evals

The behavioral contract, in executable form.

`eval_config.yaml` is **derived** from `Spec.md` §11 by `recipe-compiler`. Do not hand-edit
it to make a build pass — if an eval is wrong, the Spec that produced it is wrong.

## Two types

**`behavioral`** — the system does what was promised. Each one proves an `INT-*` or
`SPEC-*`.

**`out_of_contract`** — the system does **not** do what was excluded. Each `Intent.md` §7
exclusion should appear here.

The second type carries more weight than its share of attention. It is the executable form
of scope, and it is what catches the expansion that arrived as a helpful demo comment
nobody classified. It is also the type most often missing, because writing a test for
something that should *not* happen requires having thought about what people will ask for.

## Datasets

Put them in `evals/datasets/`. Each eval references one by path; the Build Ready gate
checks that the path resolves.

A dataset that does not actually exercise the stated behavior is a semantic gate finding —
the check passes and proves nothing.

## The test that matters

> Could every eval pass while the system is useless?

If yes, the evals measure plumbing. Derive them from what Intent promised, not from what
the implementation happens to do.
