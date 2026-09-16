# Business blueprint package

**Read-only source evidence.** Supplied by the upstream business blueprinting team.

## Rules

**Never edit anything in this directory.** Not to fix a typo, not to clarify a diagram, not
to reconcile two documents. This is the evidence base, and its value depends on being
exactly what was supplied.

**Never resolve contradictions here.** If two documents disagree, both readings get
recorded in `project_context/source_map.yaml` and raised as an `Intent.md` §10 open
question. Silently resolving a contradiction destroys the evidence that a disagreement
existed — and that disagreement is often the most valuable thing in the package.

## What goes here

Place supplied artifacts under `supplied-source-documents/`:

- Process maps, current-state documentation
- Workshop outputs and discovery notes
- Problem and value statements
- Personas and user research
- Any supporting material the blueprint team considers part of the package

## Then

Run `/intake-blueprint`. It indexes everything into
[`../project_context/source_map.yaml`](../project_context/source_map.yaml), assigns each
document a stable ID, rates its authority, and reports coverage against the `Intent.md`
sections.

Every claim in `Intent.md` then cites one of those IDs. That is what makes the business
case auditable — a reviewer can check any statement against its source.

## Authority ratings

Intake rates each document `high`, `medium`, `low` or `unclear`. An undated, unattributed
or draft-marked document is `unclear`, and using it does not make it authoritative. If the
package's central evidence is `unclear`, that is a finding to raise, not a problem to work
around.
