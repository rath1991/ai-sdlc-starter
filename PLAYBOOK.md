# The AI-Native Enterprise SDLC Playbook

One governed route from business value blueprint to prototype — with explicit return
lines when scope changes.

**Scope of this edition:** design → blueprint → package → plan → prototype → reconcile.
Test, deployment and production guardrails are acknowledged but intentionally not
elaborated here.

---

## Contents

| # | Section |
|---|---|
| 01 | [Enterprise delivery flow](#01--enterprise-delivery-flow) |
| 02 | [How a project enters the playbook](#02--how-a-project-enters-the-playbook) |
| 03 | [The governing artifact spine](#03--the-governing-artifact-spine) |
| 04 | [The three execution primitives](#04--the-three-execution-primitives) |
| 05 | [The six plays](#05--the-six-plays) |
| 06 | [Capability inventory](#06--capability-inventory) |
| 07 | [Enforcement model](#07--enforcement-model) |
| 08 | [Prototype change control](#08--prototype-change-control) |
| 09 | [Machine-readable traceability](#09--machine-readable-traceability) |
| 10 | [Repo-first adoption](#10--repo-first-adoption) |
| 11 | [Golden thread example](#11--golden-thread-example) |
| 12 | [MVP to scale](#12--mvp-to-scale) |

---

## 01 · Enterprise delivery flow

Business blueprinting remains owned upstream. After domain policy concurrence and
platform provisioning, the project bootstraps the governed core, synthesizes `Intent.md`
from the supplied business package, obtains business sign-off, then moves through
technical blueprinting, delivery and controlled reconciliation.

### Five lanes

Each lane has a distinct owner. Preserving the lanes is what preserves accountability.

**Business blueprint / intent** — *business authority owns Intent semantics*

```
BP package → load documents → AI intent draft → business review → INTENT SIGN-OFF
```

**Policy / platform** — *concurrence → environment → repository → bootstrap*

```
policy concurrence → cloud resource group → project repo → bootstrap governed core
```

**Technical blueprint** — *technical authority owns Spec semantics*

```
approved Intent → technical blueprint → Spec.md → SPEC SIGN-OFF
```

**Delivery** — *package → plan → agent-built prototype*

```
approved Spec → package (reuse/create, recipes, evals) → plan.md + build → prototype
```

**Reconciliation** — *classify → identify owner → notify → reassess*

```
stakeholder feedback → reconciliation agent → classification + owner + notification
```

### The return interchange

Three controlled back-passes. The routing agent recommends; the owning team reassesses
and signs.

| # | Route | What it means | Returns to |
|---|---|---|---|
| 1 | **Implementation only** | UI refinement, local recipe or code adjustment, execution detail. No business or solution meaning changes. | Delivery |
| 2 | **Technical blueprint change** | Notify technical blueprinting → reassess Spec → technical re-sign-off → rebuild affected downstream package. | Spec |
| 3 | **Business / scope change** | Notify business blueprinting → reassess Intent → business re-sign-off → **mandatory** downstream Spec reassessment. | Intent |

> **Dependency rule.** An Intent change makes the previous Spec baseline stale. A
> Spec-only change does not automatically reopen Intent.

### Four principles

- **Business authority stays upstream.** Business blueprinting owns the business package
  and signs every material revision of `Intent.md`.
- **Provision before project execution.** Policy concurrence, an isolated cloud
  environment and a project repository precede the workspace bootstrap.
- **Technical blueprint owns Spec.** The approved Intent is the formal design input; the
  technical blueprint team signs the resulting `Spec.md`.
- **Reconciliation routes — it does not approve.** The agent classifies, identifies
  ownership and notifies. Humans reassess and re-sign governing artifacts.

---

## 02 · How a project enters the playbook

**Business blueprinting hands over the problem. The project repository becomes the
controlled working system.**

The default operating model starts *after* an upstream team has completed workshops,
process mapping and value blueprinting. Their documentation is the source package. This
workflow obtains policy concurrence and platform provisioning, establishes the project
repository, bootstraps the governed core, then generates and governs Intent and Spec
through explicit owner sign-offs.

### Six entry steps

1. **Receive BP package** — approved working material: process maps, workshop outputs,
   problem and value statements, personas, supporting documents.
2. **Policy concurrence** — complete domain approval before project execution begins.
3. **Provision platform** — obtain the dedicated cloud resource group and project repository.
4. **Bootstrap core** — load approved agents, skills, templates and gates.
5. **Synthesize intent** — place business material under `business-blueprint/`. The AI
   reads the package, questions gaps and generates `Intent.md`.
6. **Business sign-off** — return Intent to the blueprint team. Amend ↔ review until
   approved; only then hand it to technical blueprinting.

> The repository is a governed handoff system. **It is not the creator of business
> truth.** The AI may challenge incompleteness or contradiction in the supplied
> blueprint, but unresolved business questions are routed back to the owning team. The
> approved `Intent.md` is the formal contract handed into technical blueprinting.

### Ownership

| Role | Owns | Responsibility |
|---|---|---|
| **Business blueprinting** | Intent semantics | Supplies the source package, resolves business gaps, approves initial Intent, re-signs later material revisions. |
| **Policy / platform** | Project enablement | Domain concurrence → cloud resource group → project repository → approved bootstrap. |
| **Technical blueprinting** | Spec semantics | Consumes approved Intent and enterprise standards; designs data, agents, tools, integrations, architecture; approves Spec. |
| **Delivery** | Build from approved contracts | Consumes approved Spec, derived recipes/evals and plan. Routes material learning back through reconciliation. |

### Division of labor

**What the project team does** — receive, provision, review, coordinate sign-off

- Accept the blueprint package and preserve source provenance.
- Complete policy concurrence and platform provisioning.
- Bootstrap the governed core into the project repository.
- Review AI-generated Intent before sending it upstream.
- Coordinate Intent and Spec approval cycles with owning teams.
- **Never self-approve a change simply because the prototype can implement it.**

**What AI does** — ingest, synthesize, challenge, trace, notify

- Read the blueprint folder and linked enterprise context.
- Draft Intent and expose missing or contradictory evidence.
- Run intent roast before business review.
- Build Spec from approved Intent and technical standards.
- Compile recipes, evals and traceability from approved Spec.
- Classify prototype feedback and identify the owning reassessment path.

**What the system enforces** — ownership, baseline, lineage, re-approval

- No technical blueprint from an unapproved Intent baseline.
- No delivery package from an unapproved Spec baseline.
- Intent revision → mandatory business reassessment and sign-off.
- Intent revision → downstream Spec marked for reassessment.
- Spec revision → mandatory technical reassessment and sign-off.
- The reconciliation agent can route and notify; **it cannot approve** governing changes.

---

## 03 · The governing artifact spine

**Keep the human contract small. Generate everything else.**

The playbook deliberately separates governing truth from downstream implementation
material. `Intent.md` changes only when business meaning changes. `Spec.md` changes when
solution meaning changes. Plans, recipes and code may evolve without rewriting business
truth.

| Artifact | Class | Carries |
|---|---|---|
| `Intent.md` | **Canonical** — business truth | Problem, process, pain points, personas, target outcome, value hypothesis, scope, success measures, assumptions, open questions, sign-off. |
| `Spec.md` | **Canonical** — solution truth | Behavior, architecture, agents, tools, data, contracts, enterprise constraints, reuse decisions, failure behavior, observability, eval criteria. |
| `plan.md` | **Execution** — engineering plan | Implementation order, components and files, dependencies, risks, tests and evals, sequencing, planned changes for this build. |
| `changes/CR-*.md` | **Audit** — material change record | Stakeholder feedback, classification, affected artifacts, decision, owner, rationale, and whether it is now, deferred or rejected. |

**Derived and supporting artifacts**, generated or maintained from the spine:
`traceability.yaml` · `recipes/agents/*` · `recipes/tools/*` · `evals/eval_config.yaml` ·
`decisions/ADR-*` · `project_context/*` · assistant adapters.

> **Authority rule.** Recipe contradicts Spec → Spec wins. Spec contradicts Intent →
> amend Intent or correct Spec. The implementation cannot silently redefine scope.
>
> **Volatility rule.** Moving a button does not rewrite Intent. Adding a new persona,
> decision authority or business outcome probably does.

---

## 04 · The three execution primitives

**Skills teach. Agents orchestrate. Gates enforce.**

This distinction is what avoids *agent sprawl*. A roast is usually a skill. A blueprinting
agent invokes several skills and tools. A gate decides whether the lifecycle can advance.

### Skill

Versioned institutional knowledge describing how the AI should perform a bounded task.

*Problem discovery · process mapping · intent roast · agent design standard · approved
platform services*

> Example: `intent-roast` aggressively challenges value, scope, baselines, assumptions
> and measurability.

### Agent

Orchestrates a multi-step job using several skills, enterprise context and project
artifacts.

*Intent synthesis · technical blueprint · reusable qualification · recipe compiler ·
change reconciliation*

> Agents produce or transform lifecycle artifacts. **They should not invent policy.**

### Gate

Checks that a defined contract is sufficiently complete, consistent and approved before
work advances.

*Intent Ready · Design Ready · Build Ready · Change Control*

> Semantic judgment can be agentic. Non-negotiable facts should be deterministic.

---

## 05 · The six plays

**A repeatable story every project can follow and explain.**

Each play has the same grammar: **ownership → inputs → skills/agent → artifact →
challenge → sign-off/gate.** The first play begins with an upstream blueprint package and
platform enablement; it does not recreate business blueprinting inside delivery.

### Play 01 — Receive, enable and bootstrap

*Blueprint handoff → policy concurrence → environment and repository → governed project repo*

- **Ownership.** Business blueprinting supplies business evidence. Policy and platform
  teams enable the environment. The project team accepts the package and establishes the
  governed working repository.
- **Required inputs.** Business value blueprint package · domain policy concurrence ·
  dedicated cloud resource group · provisioned project repository.
- **Bootstrap.** Pin the AI-SDLC core · load agents, skills and gates · create artifact
  folders · place blueprint documents under `business-blueprint/`.
- **Working output.** A governed project workspace, with the source package available to
  AI *with provenance*.

### Play 02 — Synthesize, challenge and sign business intent

*The blueprint package becomes the canonical project Intent*

- **Orchestrator.** `intent-synthesis` reads the supplied blueprint documents and
  enterprise context, preserves provenance, identifies gaps and drafts `Intent.md`.
  **It does not replace the business blueprinting authority.**
- **Challenge.** `grill-me` for missing evidence · `intent-roast` · baseline and value
  challenge · scope-boundary check · assumption and contradiction check.
- **Gate — business sign-off.** Return draft plus gaps to the BP team → amend and
  reassess as needed → business owner approves → version the Intent baseline. **Only
  approved Intent enters technical blueprint.**
- **Canonical output.** `Intent.md` → `DRAFT` → `CHALLENGED` → `BUSINESS REVIEW` → `APPROVED`.

### Play 03 — Blueprint the solution

*Enterprise standards + context + design reasoning*

- **Orchestrator.** `technical-blueprint` consumes approved Intent plus business and data
  context and approved enterprise patterns.
- **Skills.** technical blueprint standard · data standard · approved platform services ·
  agent architecture · tool design · integration patterns.
- **Challenge.** `architecture-roast` · "Why an agent?" · "Why this many agents?" ·
  failure, state and ownership · trace every component to Intent.
- **Canonical output.** `Spec.md` → architecture + behavior + constraints + eval criteria.

### Play 04 — Reuse before create

*Search enterprise implementation memory first*

- **Orchestrator.** `reusable-qualification` decomposes Spec into capabilities and
  searches approved reusable agents, tools, skills and patterns.
- **Reuse path.** Preserve the stable reusable core · adapt project context · apply
  project configuration · attach project evals · **avoid copy/paste forks**.
- **Create path.** Identify a genuine capability gap · generate the agent or tool recipe ·
  define contracts · define failure behavior · define success criteria.
- **Decision output.** A capability map → `REUSE` / `CREATE` → delivery packaging inputs.

### Play 05 — Compile, plan and build

*Design becomes executable engineering context*

- **Agents.** `recipe-compiler` · `planning` · developer agent.
- **Generated inputs.** Agent recipes · tool contracts · reusable plugins · eval config ·
  traceability map.
- **Loop.** Plan → build → evaluate → diagnose → modify. Failures should modify the
  **smallest affected component** while preserving the governing Spec.
- **Execution output.** `plan.md` → first prototype, with eval-driven repair.

### Play 06 — Prototype, learn, reconcile

*Feedback does not silently become scope*

- **Orchestrator.** `change-reconciliation` classifies stakeholder feedback and identifies
  exactly which governing artifact is affected.
- **Classification.** Implementation refinement · solution change · business/scope change ·
  future scope.
- **Control.** The agent proposes classification and identifies the owning team. Humans
  accept the route. The owning team reassesses and re-signs the governing artifact before
  downstream work is regenerated.
- **Audit output.** `CR-*.md` → affected artifact update → re-approval as required.

---

## 06 · Capability inventory

**Build a small reusable capability system — not dozens of bespoke bots.**

| Stage | Core skills | Primary agent | Primary artifact | Challenge / gate |
|---|---|---|---|---|
| Handoff / bootstrap | source-intake, provenance-check, project-bootstrap | Workspace bootstrap | Governed repo + blueprint package | Policy + platform readiness |
| Intent | intent-from-blueprint, grill-me, intent-roast, scope-boundary, provenance-check | Intent synthesis | `Intent.md` | Business blueprint sign-off |
| Blueprint | technical-blueprint, data-standard, platform standard, agent-architecture, tool-design, integrations, responsible AI | Technical blueprint | `Spec.md` | Architecture roast + Design Ready |
| Reuse / create | reusable-qualification, capability-decomposition, portability and coupling checks | Reusable qualification | Capability map | Reuse justification |
| Package | recipe-generation, contract-generation, eval-derivation, traceability | Recipe compiler | `recipes/*` + `evals/*` | Build Ready |
| Plan / build | planning, repo-context, implementation conventions | Planning + developer | `plan.md` + prototype | Spec-version consistency |
| Reconcile | change-classification, scope-drift detection, impact analysis | Change reconciliation | `CR-*.md` | Change Control |

### The three high-priority skills

If you build nothing else, build these.

**`grill-me`** — Keeps asking until problem, stakeholders, process, baseline, value, scope,
constraints, assumptions and unresolved questions are concrete enough to draft Intent.

**`intent-roast`** — Adversarially challenges the draft: *Why AI? Where is the baseline?
Who signed up to this? What would make the use case fail? How is success measured?*

**`architecture-roast`** — Challenges over-agentization, unclear ownership, unsupported
services, state and memory design, failure behavior, data assumptions, and orphan
components not traceable to Intent.

---

## 07 · Enforcement model

**Guidance is useful. Non-negotiables need gates.**

Use the cheapest reliable control. Let skills guide behavior, use independent agentic
reviewers for semantic judgment, and reserve deterministic checks for facts that must
never be optional.

### Level 1 — Guidance

*Skills encode institutional knowledge.*

Apply approved data standards · prefer reuse before create · challenge weak value
hypotheses · use approved architecture patterns.

### Level 2 — Agentic validation

*Independent semantic review.*

Does Spec fully address Intent? · Is the architecture over-engineered? · Has scope
drifted? · Are failures and ownership clear?

### Level 3 — Deterministic gates

*Machine-checkable invariants.*

No approved Intent → no Design Ready · no approved Spec → no packaging · unknown SPEC ID
in a recipe → fail · stale Spec version in plan → fail.

### Illustrative gate checks

**Intent Ready**

| Check | Type |
|---|---|
| Problem, personas and process are explicit | semantic |
| Value hypothesis has a baseline and a target measure | semantic |
| In-scope and out-of-scope sections exist | deterministic |
| Approved status and owner metadata present | deterministic |

**Design Ready**

| Check | Type |
|---|---|
| Every in-scope Intent item is addressed | semantic |
| Every component traces to an Intent or Spec ID | deterministic |
| Reuse / create decision completed | deterministic |
| Novel capabilities have a recipe and eval criteria | deterministic |

See [`gates/`](gates/) for the full definitions.

---

## 08 · Prototype change control

**Feedback is welcome. Silent scope drift is not.**

Every meaningful demo comment is first classified. Only changes that alter the business or
solution contract propagate upward; cosmetic implementation refinements stay local.

```
stakeholder feedback  →  change reconciliation  →  human decision
"Can we also recommend        classify + route        approve classification
 the maintenance action?"
```

### The four classifications

| | Classification | Route |
|---|---|---|
| **A** | Implementation refinement | Local delivery loop |
| **B** | Technical blueprint change | Notify technical BP team → Spec reassessment + re-sign |
| **C** | Business / scope change | Notify business BP team → Intent reassessment + re-sign → **then** Spec reassessment |
| **D** | Future scope | Change record / backlog only |

Then: generate the CR record, notify the owning team, obtain reassessment and
re-sign-off, then invalidate and regenerate affected downstream artifacts.

### Semantic ownership rule

| Feedback | Classification |
|---|---|
| "Move the button" | Implementation only |
| "Show another existing field" | Usually Spec |
| "Add another user persona" | Intent **and** Spec |
| "Use platform service B instead" | Spec |
| "Maybe forecasting later" | Future scope |

### Change record — minimum fields

Feedback or request · classification and rationale · affected Intent and Spec IDs ·
decision (accept / defer / reject) · owner and approver · resulting version change.

---

## 09 · Machine-readable traceability

**Make the reasoning chain computable.**

The goal is not paperwork. The goal is to let humans *and agents* answer: "Why does this
agent exist?", "Which business requirement does this eval prove?", and "What breaks if
this scope item changes?"

```yaml
INT-004:
  description: Reconstruct equipment failure chronology
  specs:
    - SPEC-011
    - SPEC-014

SPEC-011:
  component: FailureChronologyAgent
  recipes:
    - recipes/agents/failure_chronology.yaml
  evals:
    - EVAL-007
    - EVAL-008

EVAL-007:
  proves:
    - INT-004
    - SPEC-011
```

### Questions the system should answer

| Question | Chain |
|---|---|
| Which Intent requirement caused this component to exist? | Intent → Spec |
| Which recipe implements this Spec component? | Spec → Recipe |
| Which eval demonstrates the promised behavior? | Spec → Eval |
| Which artifacts must be regenerated after CR-017? | Impact graph |

### Artifact metadata schemas

`Intent.md` frontmatter:

```yaml
---
artifact: intent
version: 1.3
status: approved
owner: Product Owner
approved_by:
  - Business Lead
  - Design Lead
sources:
  - workshop-01
  - process-map-v2
---
```

`Spec.md` frontmatter:

```yaml
---
artifact: spec
version: 2.1
status: approved
derived_from:
  intent: 1.3
approved_by:
  - Solution Architect
---
```

The `derived_from.intent` field is what makes staleness computable: if the approved Intent
is at 1.4 and the Spec still claims derivation from 1.3, the Spec needs reassessment.

---

## 10 · Repo-first adoption

**Do not hand teams a PDF. Give them a working repository.**

The playbook is a governed product. A central repository owns the approved operating
model. After platform enablement creates the project repository, that project forks or
bootstraps the governed core — so users receive the correct workspace agents, skills,
templates and gates, while project-specific business evidence remains local.

### Central product — enterprise AI-SDLC core

*The centrally governed source of truth for the delivery system.*

Approved skills and agents · Intent/Spec/Plan/Change templates · artifact schemas and gate
logic · enterprise architecture and policy skills · reusable qualification standards ·
IDE and assistant adapters · golden-thread example and user guide · versioned releases.

### Project workspace — bootstrapped AI project

*Project truth and project-specific context live here.* The upstream blueprint package is
ingested as source evidence; corporate AI-SDLC standards are consumed, not reinvented.

`Intent.md` / `Spec.md` / `plan.md` · project changes and traceability · generated recipes
and eval configurations · project business and local context · workspace adapter for
approved AI tooling · pinned core version and provenance.

### Two distribution models

**MVP — starter / template repository.** Ship the approved skills and agents with the
project template. Explicit, inspectable, easy to demonstrate. Users clone or bootstrap and
begin immediately. *Best for: pilot, controlled rollout, proving adoption, refining the
asset set.*

**Mature — centrally versioned core + thin project adapter.** Projects pin a governed
release rather than maintaining copied forks. Project-local files contain context and
artifacts; central standards evolve once. *Prevents: project A running grill-me v1 while
project B silently modifies its own architecture rules.*

> **Tool-neutral principle.** Define corporate assets as AI-SDLC **skills**, **agents** and
> **gates**. IDE-specific folders are distribution adapters, not the methodology. The
> semantics of Intent, Spec, change reconciliation and gates remain constant across any
> approved enterprise toolchain.

---

## 11 · Golden thread example

**Teach the playbook with one problem from start to finish.**

An equipment-reliability use case acts as the running example. The point is not the
domain — it is to show how one vague idea becomes a governed artifact chain, and how
feedback is reconciled without losing traceability.

| Stage | What happens |
|---|---|
| **Vague idea** | "Use AI to identify equipment reliability risks." |
| **Grill me** | Who uses it? Which process? What signals? What decision? What value? |
| **Intent** | Approved scope: identify leading risk and reconstruct evidence — *not* prescribe maintenance. |
| **Spec + reuse** | Reuse retrieval, data quality and tooling; create only the genuinely novel chronology capability. |
| **Prototype** | Agents build from approved Spec, recipes, context and eval criteria. |
| **Feedback** | "Add maintenance recommendation." Classified as scope expansion → CR → defer or amend Intent. |

### What each challenge caught

**Intent roast found a weak value statement.** "Improve reliability" became a measurable
operational outcome with a baseline, a target and a decision owner.

**Architecture roast found an unnecessary agent.** A deterministic transformation was
removed from the agent graph and implemented as a tool.

**Reconciliation found prototype scope drift.** The recommendation feature was not slipped
into code — it became an explicit business decision.

See [`examples/golden-thread/`](examples/golden-thread/) for the worked artifacts.

---

## 12 · MVP to scale

**Start with the minimum executable playbook.**

Adoption comes from usefulness, not methodology training. Build the artifacts and
challenger skills first, prove them on one project, then expand the enterprise memory and
automation layers.

| Phase | Build |
|---|---|
| **1 — Artifact contracts** | Intent, Spec, Plan, Change templates + schemas + metadata. |
| **2 — Design skills** | `grill-me`, `intent-roast`, `technical-blueprint`, `architecture-roast`. |
| **3 — Core agents** | Intent synthesis, technical blueprint, recipe compiler. |
| **4 — Trace + gates** | Traceability generator, Intent Ready, Design Ready, Build Ready. |
| **5 — Scale intelligence** | Reusable qualification, enterprise context service, change reconciliation, richer eval automation. |

### Definition of adoption

| Role | Can do |
|---|---|
| **Consultant** | Ingest the blueprint package, generate challenged Intent, coordinate sign-off, and knows when a later change requires reassessment. |
| **Architect** | Generate and challenge Spec using enterprise standards and reuse-first design. |
| **Developer** | Start from an approved delivery package and plan — not from informal chat instructions. |
| **Leadership** | Trace value → architecture → implementation evidence without reading source code. |

### Adoption measures

- **Activation.** Percentage of new projects bootstrapped from the approved core rather
  than assembled manually.
- **Artifact discipline.** Percentage of delivery work beginning from an approved Intent +
  Spec baseline with valid traceability.
- **Standard drift.** Number of project-local forks or overrides of central skills and
  agents. Target should trend toward zero.

---

*Synthesizes artifact-driven AI-native SDLC practices with enterprise business value
blueprinting, technical blueprinting, enterprise context, reuse-first design, builder
recipes, agents-building-agents and controlled prototype learning.*
